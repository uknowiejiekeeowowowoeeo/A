
--Flashlight -- SCRIPT NOT MADE BY ME
function sandbox(var,func)
	local env = getfenv(func)
	local newenv = setmetatable({},{
		__index = function(self,k)
			if k=="script" then
				return var
			else
				return env[k]
			end
		end,
	})
	setfenv(func,newenv)
	return func
end
cors = {}
mas = Instance.new("Model",game:GetService("Lighting"))
Tool0 = Instance.new("Tool")
Part1 = Instance.new("Part")
Sound2 = Instance.new("Sound")
SpecialMesh3 = Instance.new("SpecialMesh")
SpotLight4 = Instance.new("SpotLight")
LocalScript5 = Instance.new("LocalScript")
LocalScript6 = Instance.new("LocalScript")
ModuleScript7 = Instance.new("ModuleScript")
Tool0.Name = "Flashlight"
Tool0.Parent = mas
Tool0.TextureId = "http://www.roblox.com/asset/?id=115955232"
Tool0.Grip = CFrame.new(0.100000001, -0.400000006, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1)
Tool0.GripPos = Vector3.new(0.10000000149011612, -0.4000000059604645, 0)
Tool0.ToolTip = "Flashlight"
Part1.Name = "Handle"
Part1.Parent = Tool0
Part1.CFrame = CFrame.new(10.9154053, 0.25, -18.9892578, 0, 0, 1, 0, 1, 0, -1, 0, 0)
Part1.Orientation = Vector3.new(0, 90, 0)
Part1.Position = Vector3.new(10.9154052734375, 0.25, -18.9892578125)
Part1.Rotation = Vector3.new(0, 90, 0)
Part1.Color = Color3.new(0.960784, 0.803922, 0.188235)
Part1.Size = Vector3.new(0.5, 0.5, 2)
Part1.BottomSurface = Enum.SurfaceType.Smooth
Part1.BrickColor = BrickColor.new("Bright yellow")
Part1.TopSurface = Enum.SurfaceType.Smooth
Part1.brickColor = BrickColor.new("Bright yellow")
Part1.FormFactor = Enum.FormFactor.Custom
Part1.formFactor = Enum.FormFactor.Custom
Sound2.Parent = Part1
Sound2.SoundId = "http://www.roblox.com/asset/?id=115959318"
Sound2.Volume = 1
SpecialMesh3.Parent = Part1
SpecialMesh3.MeshId = "http://www.roblox.com/asset/?id=115955313"
SpecialMesh3.Scale = Vector3.new(0.699999988079071, 0.699999988079071, 0.699999988079071)
SpecialMesh3.TextureId = "http://www.roblox.com/asset?id=115955343"
SpecialMesh3.MeshType = Enum.MeshType.FileMesh
SpotLight4.Parent = Part1
SpotLight4.Enabled = false
SpotLight4.Range = 40
SpotLight4.Brightness = 10
SpotLight4.Shadows = true
LocalScript5.Parent = Tool0
table.insert(cors,sandbox(LocalScript5,function()
player = game.Players.LocalPlayer
tool = script.Parent

repeat wait() until player

tool.Equipped:connect(function(mouse)
	mouse.Button1Down:connect(function()
		tool.Handle.SpotLight.Enabled = not tool.Handle.SpotLight.Enabled
		tool.Handle.Sound:Play()
	end)
end)
end))
LocalScript6.Parent = Tool0
table.insert(cors,sandbox(LocalScript6,function()
-- Variables for services
local render = game:GetService("RunService").RenderStepped
local contextActionService = game:GetService("ContextActionService")
local userInputService = game:GetService("UserInputService")

local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local Tool = script.Parent

-- Variables for Module Scripts
local screenSpace = require(Tool:WaitForChild("ScreenSpace"))

local connection

local neck, shoulder, oldNeckC0, oldShoulderC0

local mobileShouldTrack = true

-- Thourough check to see if a character is sitting
local function amISitting(character)
	local t = character.Torso
	for _, part in pairs(t:GetConnectedParts(true)) do
		if part:IsA("Seat") or part:IsA("VehicleSeat") then
			return true
		end
	end
end

-- Function to call on renderstepped. Orients the character so it is facing towards
-- the player mouse's position in world space. If character is sitting then the torso
-- should not track
local function frame(mousePosition)
	-- Special mobile consideration. We don't want to track if the user was touching a ui
	-- element such as the movement controls. Just return out of function if so to make sure
	-- character doesn't track
	if not mobileShouldTrack then return end
	
	-- Make sure character isn't swiming. If the character is swimming the following code will
	-- not work well; the character will not swim correctly. Besides, who shoots underwater?
	if player.Character.Humanoid:GetState() ~= Enum.HumanoidStateType.Swimming then
		local torso = player.Character.Torso
		local head = player.Character.Head
		
		local toMouse = (mousePosition - head.Position).unit
		local angle = math.acos(toMouse:Dot(Vector3.new(0,1,0)))
		
		local neckAngle = angle
	
		-- Limit how much the head can tilt down. Too far and the head looks unnatural
		if math.deg(neckAngle) > 110 then
			neckAngle = math.rad(110)
		end
		neck.C0 = CFrame.new(0,1,0) * CFrame.Angles(math.pi - neckAngle,math.pi,0)
		
		-- Calculate horizontal rotation
		local arm = player.Character:FindFirstChild("Right Arm")
		local fromArmPos = torso.Position + torso.CFrame:vectorToWorldSpace(Vector3.new(
			torso.Size.X/2 + arm.Size.X/2, torso.Size.Y/2 - arm.Size.Z/2, 0))
		local toMouseArm = ((mousePosition - fromArmPos) * Vector3.new(1,0,1)).unit
		local look = (torso.CFrame.lookVector * Vector3.new(1,0,1)).unit
		local lateralAngle = math.acos(toMouseArm:Dot(look))		
		
		-- Check for rogue math
		if tostring(lateralAngle) == "-1.#IND" then
			lateralAngle = 0
		end		
		
		-- Handle case where character is sitting down
		if player.Character.Humanoid:GetState() == Enum.HumanoidStateType.Seated then			
			
			local cross = torso.CFrame.lookVector:Cross(toMouseArm)
			if lateralAngle > math.pi/2 then
				lateralAngle = math.pi/2
			end
			if cross.Y < 0 then
				lateralAngle = -lateralAngle
			end
		end	
		
		-- Turn shoulder to point to mouse
		shoulder.C0 = CFrame.new(1,0.5,0) * CFrame.Angles(math.pi/2 - angle,math.pi/2 + lateralAngle,0)	
		
		-- If not sitting then aim torso laterally towards mouse
		if not amISitting(player.Character) then
			torso.CFrame = CFrame.new(torso.Position, torso.Position + (Vector3.new(
				mousePosition.X, torso.Position.Y, mousePosition.Z)-torso.Position).unit)
		end	
	end
end

-- Function to bind to render stepped if player is on PC
local function pcFrame()
	frame(mouse.Hit.p)
end

-- Function to bind to touch moved if player is on mobile
local function mobileFrame(touch, processed)
	-- Check to see if the touch was on a UI element. If so, we don't want to update anything
	if not processed then
		-- Calculate touch position in world space. Uses Stravant's ScreenSpace Module script
		-- to create a ray from the camera.
		local test = screenSpace.ScreenToWorld(touch.Position.X, touch.Position.Y, 1)
		local nearPos = game.Workspace.CurrentCamera.CoordinateFrame:vectorToWorldSpace(screenSpace.ScreenToWorld(touch.Position.X, touch.Position.Y, 1))
		nearPos = game.Workspace.CurrentCamera.CoordinateFrame.p - nearPos
		local farPos = screenSpace.ScreenToWorld(touch.Position.X, touch.Position.Y,50) 
		farPos = game.Workspace.CurrentCamera.CoordinateFrame:vectorToWorldSpace(farPos) * -1
		if farPos.magnitude > 900 then
			farPos = farPos.unit * 900
		end
		local ray = Ray.new(nearPos, farPos)
		local part, pos = game.Workspace:FindPartOnRay(ray, player.Character)
		
		-- if a position was found on the ray then update the character's rotation
		if pos then
			frame(pos)
		end
	end
end

local function OnActivated()
	local myModel = player.Character
	if Tool.Enabled and myModel and myModel:FindFirstChild('Humanoid') and myModel.Humanoid.Health > 0 then
		Tool.Enabled = false
		game.ReplicatedStorage.ROBLOX_RocketFireEvent:FireServer(mouse.Hit.p)
		wait(2)

		Tool.Enabled = true
	end
end

local oldIcon = nil
-- Function to bind to equip event
local function equip()
	local torso = player.Character.Torso
	
	-- Setup joint variables
	neck = torso.Neck	
	oldNeckC0 = neck.C0
	shoulder = torso:FindFirstChild("Right Shoulder")
	oldShoulderC0 = shoulder.C0
	
	-- Remember old mouse icon and update current
	oldIcon = mouse.Icon
	mouse.Icon = "rbxasset://textures\\GunCursor.png"
	
	-- Bind TouchMoved event if on mobile. Otherwise connect to renderstepped
	if userInputService.TouchEnabled then
		connection = userInputService.TouchMoved:connect(mobileFrame)
	else
		connection = render:connect(pcFrame)
	end
	
	-- Bind TouchStarted and TouchEnded. Used to determine if character should rotate
	-- during touch input
	userInputService.TouchStarted:connect(function(touch, processed)
		mobileShouldTrack = not processed
	end)	
	userInputService.TouchEnded:connect(function(touch, processed)
		mobileShouldTrack = false
	end)

	-- If game uses filtering enabled then need to update server while tool is
	-- held by character.
	if workspace.FilteringEnabled then
		while connection do
			wait()
			game.ReplicatedStorage.ROBLOX_RocketUpdateEvent:FireServer(neck.C0, shoulder.C0)
		end
	end
end

-- Function to bind to Unequip event
local function unequip()
	if connection then connection:disconnect() end
	
	mouse.Icon = oldIcon
	
	neck.C0 = oldNeckC0
	shoulder.C0 = oldShoulderC0
end

-- Bind tool events
Tool.Equipped:connect(equip)
Tool.Unequipped:connect(unequip)
Tool.Activated:connect(OnActivated)
end))
ModuleScript7.Name = "ScreenSpace"
ModuleScript7.Parent = Tool0
table.insert(cors,sandbox(ModuleScript7,function()
local PlayerMouse = Game:GetService('Players').LocalPlayer:GetMouse()

local ScreenSpace = {}

-- Getter functions, with a couple of hacks for Ipad pre-focus.
function ScreenSpace.ViewSizeX()
	local x = PlayerMouse.ViewSizeX
	local y = PlayerMouse.ViewSizeY
	if x == 0 then
		return 1024
	else
		if x > y then
			return x
		else
			return y
		end
	end
end

function ScreenSpace.ViewSizeY()
	local x = PlayerMouse.ViewSizeX
	local y = PlayerMouse.ViewSizeY
	if y == 0 then
		return 768
	else
		if x > y then
			return y
		else
			return x
		end
	end
end

-- Nice getter for aspect ratio. Due to the checks in the ViewSize functions this
-- will never fail with a divide by zero error.
function ScreenSpace.AspectRatio()
	return ScreenSpace.ViewSizeX() / ScreenSpace.ViewSizeY()
end

-- WorldSpace -> ScreenSpace. Raw function taking a world position and giving you the
-- screen position.
function ScreenSpace.WorldToScreen(at)
	local point = Workspace.CurrentCamera.CoordinateFrame:pointToObjectSpace(at)
	local aspectRatio = ScreenSpace.AspectRatio()
	local hfactor = math.tan(math.rad(Workspace.CurrentCamera.FieldOfView)/2)
	local wfactor = aspectRatio*hfactor
	--
	local x = (point.x/point.z) / -wfactor
	local y = (point.y/point.z) /  hfactor
	--
	return Vector2.new(ScreenSpace.ViewSizeX()*(0.5 + 0.5*x), ScreenSpace.ViewSizeY()*(0.5 + 0.5*y))
end

-- ScreenSpace -> WorldSpace. Raw function taking a screen position and a depth and 
-- converting it into a world position.
function ScreenSpace.ScreenToWorld(x, y, depth)
	local aspectRatio = ScreenSpace.AspectRatio()
	local hfactor = math.tan(math.rad(Workspace.CurrentCamera.FieldOfView)/2)
	local wfactor = aspectRatio*hfactor
	--
	local xf, yf = x/ScreenSpace.ViewSizeX()*2 - 1, y/ScreenSpace.ViewSizeY()*2 - 1
	local xpos = xf * -wfactor * depth
	local ypos = yf *  hfactor * depth
	--
	return Vector3.new(xpos, ypos, depth)
end

-- ScreenSize -> WorldSize
function ScreenSpace.ScreenWidthToWorldWidth(screenWidth, depth)	
	local aspectRatio = ScreenSpace.AspectRatio()
	local hfactor = math.tan(math.rad(Workspace.CurrentCamera.FieldOfView)/2)
	local wfactor = aspectRatio*hfactor
	local sx = ScreenSpace.ViewSizeX()
	--
	return -(screenWidth / sx) * 2 * wfactor * depth
end
function ScreenSpace.ScreenHeightToWorldHeight(screenHeight, depth)
	local hfactor = math.tan(math.rad(Workspace.CurrentCamera.FieldOfView)/2)
	local sy = ScreenSpace.ViewSizeY()
	--
	return -(screenHeight / sy) * 2 * hfactor * depth
end

-- WorldSize -> ScreenSize
function ScreenSpace.WorldWidthToScreenWidth(worldWidth, depth)
	local aspectRatio = ScreenSpace.AspectRatio()
	local hfactor = math.tan(math.rad(Workspace.CurrentCamera.FieldOfView)/2)
	local wfactor = aspectRatio*hfactor
	local sx = ScreenSpace.ViewSizeX()
	--
	return -(worldWidth * sx) / (2 * wfactor * depth)
end
function ScreenSpace.WorldHeightToScreenHeight(worldHeight, depth)
	local hfactor = math.tan(math.rad(Workspace.CurrentCamera.FieldOfView)/2)
	local sy = ScreenSpace.ViewSizeY()
	--
	return -(worldHeight * sy) / (2 * hfactor * depth)
end

-- WorldSize + ScreenSize -> Depth needed
function ScreenSpace.GetDepthForWidth(screenWidth, worldWidth)
	local aspectRatio = ScreenSpace.AspectRatio()
	local hfactor = math.tan(math.rad(Workspace.CurrentCamera.FieldOfView)/2)
	local wfactor = aspectRatio*hfactor
	local sx, sy = ScreenSpace.ViewSizeX(), ScreenSpace.ViewSizeY()
	--
	return -(sx * worldWidth) / (screenWidth * 2 * wfactor)	
end
function ScreenSpace.GetDepthForHeight(screenHeight, worldHeight)
	local hfactor = math.tan(math.rad(Workspace.CurrentCamera.FieldOfView)/2)
	local sy = ScreenSpace.ViewSizeY()
	--
	return -(sy * worldHeight) / (screenHeight * 2 * hfactor)	
end

-- ScreenSpace -> WorldSpace. Taking a screen height, and a depth to put an object 
-- at, and returning a size of how big that object has to be to appear that size
-- at that depth.
function ScreenSpace.ScreenToWorldByHeightDepth(x, y, screenHeight, depth)
	local aspectRatio = ScreenSpace.AspectRatio()
	local hfactor = math.tan(math.rad(Workspace.CurrentCamera.FieldOfView)/2)
	local wfactor = aspectRatio*hfactor
	local sx, sy = ScreenSpace.ViewSizeX(), ScreenSpace.ViewSizeY()
	--
	local worldHeight = -(screenHeight/sy) * 2 * hfactor * depth
	--
	local xf, yf = x/sx*2 - 1, y/sy*2 - 1
	local xpos = xf * -wfactor * depth
	local ypos = yf *  hfactor * depth
	--
	return Vector3.new(xpos, ypos, depth), worldHeight
end

-- ScreenSpace -> WorldSpace. Taking a screen width, and a depth to put an object 
-- at, and returning a size of how big that object has to be to appear that size
-- at that depth.
function ScreenSpace.ScreenToWorldByWidthDepth(x, y, screenWidth, depth)
	local aspectRatio = ScreenSpace.AspectRatio()
	local hfactor = math.tan(math.rad(Workspace.CurrentCamera.FieldOfView)/2)
	local wfactor = aspectRatio*hfactor
	local sx, sy = ScreenSpace.ViewSizeX(), ScreenSpace.ViewSizeY()
	--
	local worldWidth = (screenWidth/sx) * 2 * -wfactor * depth
	--
	local xf, yf = x/sx*2 - 1, y/sy*2 - 1
	local xpos = xf * -wfactor * depth
	local ypos = yf *  hfactor * depth
	--
	return Vector3.new(xpos, ypos, depth), worldWidth
end

-- ScreenSpace -> WorldSpace. Taking a screen height that you want that object to be
-- and a world height that is the size of that object, and returning the position to
-- put that object at to satisfy those.
function ScreenSpace.ScreenToWorldByHeight(x, y, screenHeight, worldHeight)
	local aspectRatio = ScreenSpace.AspectRatio()
	local hfactor = math.tan(math.rad(Workspace.CurrentCamera.FieldOfView)/2)
	local wfactor = aspectRatio*hfactor
	local sx, sy = ScreenSpace.ViewSizeX(), ScreenSpace.ViewSizeY()
	--
	local depth = - (sy * worldHeight) / (screenHeight * 2 * hfactor)
	--
	local xf, yf = x/sx*2 - 1, y/sy*2 - 1
	local xpos = xf * -wfactor * depth
	local ypos = yf *  hfactor * depth
	--
	return Vector3.new(xpos, ypos, depth)
end

-- ScreenSpace -> WorldSpace. Taking a screen width that you want that object to be
-- and a world width that is the size of that object, and returning the position to
-- put that object at to satisfy those.
function ScreenSpace.ScreenToWorldByWidth(x, y, screenWidth, worldWidth)
	local aspectRatio = ScreenSpace.AspectRatio()
	local hfactor = math.tan(math.rad(Workspace.CurrentCamera.FieldOfView)/2)
	local wfactor = aspectRatio*hfactor
	local sx, sy = ScreenSpace.ViewSizeX(), ScreenSpace.ViewSizeY()
	--
	local depth = - (sx * worldWidth) / (screenWidth * 2 * wfactor)
	--
	local xf, yf = x/sx*2 - 1, y/sy*2 - 1
	local xpos = xf * -wfactor * depth
	local ypos = yf *  hfactor * depth
	--
	return Vector3.new(xpos, ypos, depth)
end

return ScreenSpace




end))
for i,v in pairs(mas:GetChildren()) do
	v.Parent = game:GetService("Players").LocalPlayer.Backpack
	pcall(function() v:MakeJoints() end)
end
mas:Destroy()
for i,v in pairs(cors) do
	spawn(function()
		pcall(v)
	end)
end
