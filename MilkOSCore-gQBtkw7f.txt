-- MilkOS OWNED AND MAINTAINED BY Milk_man_3000
 
-- Variables
local coreVersion = "Alpha 0.1";
local isBeta = true;
 
-- Functions
function checkApiAndLoad(path)
 if fs.exists(path) == true then
  term.setTextColor(colors.pink);
  write("[APIS] API ".. path .. " finded, ".. fs.getSize(path).. "b on HDD, reading... ");
  term.setTextColor(colors.green);
  os.loadAPI(path);
  print("done");
  term.setTextColor(colors.white);
 else
  term.setTextColor(colors.red);
  print("[ !! ] API ".. path .. " not found, loading canceled!");
  term.setTextColor(colors.white);
 end
end
 
function unloadApi(path)
 term.setTextColor(colors.pink);
 write("[APIS] API ".. path .." is in unloading... ");
 if fs.exists(path) == true then
  term.setTextColor(colors.green);
  print("done");
  term.setTextColor(colors.white);
 else
  term.setTextColor(colors.red);
  print("failed!");
  term.setTextColor(colors.white);
 end
end

function getRawAndSave(pastebinCode, savePath)
 write("[WORK] Downloading ".. pastebinCode .. "... ");
 local downloadRequest = http.get("http://www.pastebin.com/raw/".. pastebinCode);
 local fileOutput = downloadRequest.readAll();
 local fileWriter = fs.open(savePath, "w");
 fileWriter.write(fileOutput);
 fileWriter.close();
 print("done.");
end
-- Core Main
term.clear();
term.setCursorPos(1, 1);
-- Put your APIS here:
checkApiAndLoad("/milkos/apis/messages");
checkApiAndLoad("/milkos/apis/stringapi");
checkApiAndLoad("/milkos/apis/filesystem");
-- End of APIS list to load
 
 
-- Begin console
messages.info("Begin Console on computer ".. os.getComputerID() .. " at ".. os.clock());
 
if isBeta == true then
 messages.warn("This is the ".. coreVersion .. " version of MilkOS");
end
while true do
 write(os.getComputerID() .. "@/" .. shell.dir() .. ">");
 local commandCore = read();
 local commandArgs = stringapi.split(commandCore, " ");
-- commands
 if commandArgs[1] == "launch" then
  if commandArgs[2] == nil then
   print("Usage: launch <arg> <pgrm>")
   else
  if commandArgs[2] == "-r" then
   	if commandArgs[3] == nil then
   	 print("Usage: launch -r <pgrm>")
   	else
   	 messages.info("Grabbing program...");
   	 shell.run("/milkos/pgrms/".. commandArgs[3] .."");
    end
  else
  if commandArgs[2] == "-o" then
  	if commandArgs[3] == nil then
  	 print("Usage: launch -o <path_to_program>")
  	else
  	 messages.info("Grabbing program...");
  	 shell.run("".. commandArgs[3] .."");
  	end
  end
end
end
end
if commandArgs[1] == "reboot" then
 write("Are you sure? ([y]es/[n]o)?:")
 local rebootQ = read()
 if rebootQ == "y" or rebootQ == "Y" then
  print("Shutting down");
  sleep(3)
  os.reboot()
 else
 end
end
if commandArgs[1] == "wget" then
   if commandArgs[2] == nil then
    print("Usage: wget <url> <path>");
   else
    if commandArgs[3] == nil then
     print("Usage: wget ".. commandArgs[2] .. " <path>");
    else
     messages.info("Checking \"".. commandArgs[2] .. "\"...");
     local success, message = http.checkURL(commandArgs[2]);
     if not success then
      messages.warn("The URL is invalid: ".. message);
     else
      messages.work("Downloading ".. commandArgs[2] .. " on " .. commandArgs[3].. "...");
      local fileGet = http.get(commandArgs[2]);
      messages.info("Reading URL file...");
      local fileOut = fileGet.readAll();
      local fileWrt = fs.open(commandArgs[3], "w");
      messages.work("Writing on \"" .. commandArgs[3] .. "\"");
      fileWrt.write(fileOut);
      fileWrt.close();
      messages.done("Writing on \"" .. commandArgs[3] .. "\" done.");
     end
 end
end

else if commandArgs[1] == "dir" or commandArgs[1] == "ls" then
    if commandArgs[2] == nil then
	 print("Directory of \"/".. shell.dir() .. "\"");
	 filesystem.listFiles(shell.dir());
    else
	 print("Directory of \"/".. commandArgs[2] .. "\"");
	 filesystem.listFiles(commandArgs[2]);
    end
    else if commandArgs[1] == "cd" then
     local newPath = shell.resolve(commandArgs[2]);
     if fs.isDir(newPath) and fs.exists(newPath) then
	  shell.setDir(newPath);
	  messages.done("Successfuly changed to \"/".. shell.dir() .. "\"");
     else
  	  messages.suggest("Please enter a valid directory.");
 end
else if commandArgs[1] == "update" then 
 shell.run("/programs/github clone milk-man-3000/milkos milkos");
end







end
end
end
end
