if game:service('RunService'):IsStudio() then print('!STUDIO!') else
	if game:service('CoreGui'):findFirstChild('LogHolder') then return nil
	end
end

local LogHolder = Instance.new("ScreenGui")
local Logs = Instance.new("Frame")
local Scroll = Instance.new("ScrollingFrame")
local Template = Instance.new("TextLabel")

LogHolder.Name = "LogHolder"
if game:service('RunService'):IsStudio() then LogHolder.Parent = game.Players.LocalPlayer.PlayerGui else
	LogHolder.Parent = game.CoreGui
end

Logs.Name = "Logs"
Logs.Parent = LogHolder
Logs.AnchorPoint = Vector2.new(0.5, 0.5)
Logs.BackgroundColor3 = Color3.new(1, 1, 1)
Logs.Position = UDim2.new(0.200000003, 0, 0.200000003, 0)
Logs.Size = UDim2.new(0, 400, 0, 250)
Logs.Style = Enum.FrameStyle.DropShadow

Scroll.Name = "Scroll"
Scroll.Parent = Logs
Scroll.BackgroundColor3 = Color3.new(1, 1, 1)
Scroll.BackgroundTransparency = 1
Scroll.BorderSizePixel = 0
Scroll.Size = UDim2.new(1, 0, 1, 0)
Scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
Scroll.ScrollBarThickness = 6

Template.Name = "Template"
Template.Parent = Logs
Template.BackgroundColor3 = Color3.new(1, 1, 1)
Template.BackgroundTransparency = 1
Template.Position = UDim2.new(0, 0, 0, -25)
Template.Size = UDim2.new(1, 0, 0, 20)
Template.Font = Enum.Font.ArialBold
Template.Text = ""
Template.TextColor3 = Color3.new(1, 1, 1)
Template.TextSize = 15
Template.TextXAlignment = Enum.TextXAlignment.Left
Template.TextWrap = true

Logs.Active = true
Logs.Draggable = true

local loggedTable = {}

local getTotalSize = function()
local totalSize = UDim2.new(0, 0, 0, 0)
	
	for i, v in next, loggedTable do
		totalSize = totalSize + UDim2.new(0, 0, 0, v.Size.Y.Offset)
	end
	
	return totalSize
end

local BUD = UDim2.new(0, 0, 0, 0)
local TotalNum = 0

local function GenLog(txt, colo, time)
	local oldColo = Color3.fromRGB(0, 0, 0)	
	
	local Temp = Template:Clone()
	Temp.Parent = Scroll
	Temp.Name = txt..'Logged'
	Temp.Text = tostring(txt)
	Temp.Visible = true
	Temp.Position = BUD + UDim2.new(0, 0, 0, 0)
	if colo then oldColo = colo Temp.TextColor3 = colo elseif not colo then Temp.TextColor3 = Color3.fromRGB(200, 200, 200) end

	local timeVal = Instance.new('StringValue', Temp)
	timeVal.Name = 'TimeVal'
	timeVal.Value = time

	TotalNum = TotalNum + 1
	
	if not Temp.TextFits then repeat Temp.Size = UDim2.new(Temp.Size.X.Scale, Temp.Size.X.Offset, Temp.Size.Y.Scale, Temp.Size.Y.Offset + 10)
		Temp.Text = txt
	until Temp.TextFits 
end

	BUD = BUD + UDim2.new(0, 0, 0, Temp.Size.Y.Offset)
	
	table.insert(loggedTable, Temp)
	
	local totSize = getTotalSize()
	
	if totSize.Y.Offset >= Scroll.CanvasSize.Y.Offset then Scroll.CanvasSize = UDim2.new(totSize.X.Scale, totSize.X.Offset, totSize.Y.Scale, totSize.Y.Offset + 100)
	Scroll.CanvasPosition = Scroll.CanvasPosition + Vector2.new(0, totSize.Y.Offset) 
	end
	
	return Temp
end

local ChatData = ""

local function SaveToFile()
	local t = os.date("*t")
	local dateDat = t['hour']..' '..t['min']..' '..t['sec']..' '..t['day']..'.'..t['month']..'.'..t['year']
	
	ChatData = ""
	
	for i, v in pairs(Scroll:GetChildren()) do
		ChatData = ChatData..v.TimeVal.Value..' '..v.Text..'\n'
	end
	
	writefile('ChatLogs '..dateDat..'.txt', ChatData)
end


local function Clear()
	loggedTable = {}
	ChatData = ""
	Scroll.CanvasPosition = Vector2.new(0, 0)
	for i, v in pairs(Scroll:GetChildren()) do
		v:Destroy()
	end
	Scroll.CanvasSize = UDim2.new(0, 0, 0, 0)
	BUD = UDim2.new(0, 0, 0, 0)
end

local LogPlr = function(plr)
			plr.Chatted:connect(function(msg)
				
			local t = os.date("*t")
			local dateDat = t['hour']..':'..t['min']..':'..t['sec']
	
			if string.len(msg) >= 1000 then return nil end
			if string.lower(msg) == 'clear' and plr == game:service('Players').LocalPlayer then Clear() return nil end
			if string.lower(msg) == 'savetofile' and plr == game:service('Players').LocalPlayer then SaveToFile() return nil end
			if string.sub(msg, 1, 1):match('%p') and string.sub(msg, 2, 2):match('%a') and string.len(msg) >= 5 then GenLog(plr.Name..': '..msg, Color3.new(255, 0, 0), dateDat) else
			GenLog(plr.Name..': '..msg, Color3.new(255, 255, 255), dateDat)
			end
	end)
end

for i, v in pairs(game.Players:GetChildren()) do
	LogPlr(v)
end

game.Players.PlayerAdded:connect(function(plr)
	LogPlr(plr)
end)
