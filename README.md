-- CONFIG
local CustomConfig = {
    Position = getgenv().Position or UDim2.new(0.01, 0, 0.08, 0),
    OnTop = getgenv().OnTop or false
}

-------------------------------------------------------------------------

-- [Race Status GUI v8 - Gears + Buy Gears Fragments]
repeat task.wait() until game:IsLoaded() and game.Players.LocalPlayer:FindFirstChild("DataLoaded")

if game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("Main (minimal)") then
	repeat task.wait()
	until not game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("Main (minimal)")
end

local CoreGui = game:GetService("CoreGui")
pcall(function()
	if CoreGui:FindFirstChild("CustomGUI_93063325506256") then
		CoreGui.CustomGUI_93063325506256:Destroy()
	end
end)

local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = CoreGui
ScreenGui.Name = "CustomGUI_93063325506256"
ScreenGui.ResetOnSpawn = false

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 220, 0, 175)
MainFrame.Position = CustomConfig.Position
MainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
MainFrame.BorderSizePixel = 0
MainFrame.ClipsDescendants = true
MainFrame.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(0, 10)
UICorner.Parent = MainFrame

local UIGradient = Instance.new("UIGradient")
UIGradient.Color = ColorSequence.new{
	ColorSequenceKeypoint.new(0, Color3.fromRGB(255, 0, 0)),
	ColorSequenceKeypoint.new(0.166, Color3.fromRGB(255, 255, 0)),
	ColorSequenceKeypoint.new(0.333, Color3.fromRGB(0, 255, 0)),
	ColorSequenceKeypoint.new(0.5, Color3.fromRGB(0, 255, 255)),
	ColorSequenceKeypoint.new(0.666, Color3.fromRGB(255, 0, 255)),
	ColorSequenceKeypoint.new(0.833, Color3.fromRGB(255, 0, 127)),
	ColorSequenceKeypoint.new(1, Color3.fromRGB(255, 102, 0))
}
UIGradient.Rotation = 45

local Stroke = Instance.new("UIStroke")
Stroke.Thickness = 3
Stroke.Color = Color3.fromRGB(255, 255, 255)
Stroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
Stroke.Parent = MainFrame
UIGradient.Parent = Stroke

task.spawn(function()
	while true do
		game:GetService("TweenService")
			:Create(UIGradient, TweenInfo.new(3, Enum.EasingStyle.Linear), {Rotation = 360})
			:Play()
		task.wait(3)
		UIGradient.Rotation = 0
	end
end)

local ContentFrame = Instance.new("Frame")
ContentFrame.Size = UDim2.new(1, -10, 1, -10)
ContentFrame.Position = UDim2.new(0, 5, 0, 5)
ContentFrame.BackgroundTransparency = 1
ContentFrame.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 6)
UIListLayout.HorizontalAlignment = Enum.HorizontalAlignment.Center
UIListLayout.VerticalAlignment = Enum.VerticalAlignment.Center
UIListLayout.Parent = ContentFrame

-- Username
local UsernameLabel = Instance.new("TextLabel", ContentFrame)
UsernameLabel.Size = UDim2.new(1, 0, 0, 26)
UsernameLabel.BackgroundTransparency = 1
UsernameLabel.Text = "..."
UsernameLabel.TextColor3 = Color3.fromRGB(0, 170, 255)
UsernameLabel.Font = Enum.Font.GothamBold
UsernameLabel.TextSize = 22

-- Status
local TrainingLabel = Instance.new("TextLabel", ContentFrame)
TrainingLabel.Size = UDim2.new(1, 0, 0, 26)
TrainingLabel.BackgroundTransparency = 1
TrainingLabel.Text = "Training Race V4"
TrainingLabel.TextColor3 = Color3.fromRGB(255, 200, 0)
TrainingLabel.Font = Enum.Font.GothamBold
TrainingLabel.TextSize = 22

-- Tier
local TierLabel = Instance.new("TextLabel", ContentFrame)
TierLabel.Size = UDim2.new(1, 0, 0, 26)
TierLabel.BackgroundTransparency = 1
TierLabel.Text = "Tiers - V4 : ..."
TierLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
TierLabel.Font = Enum.Font.GothamBold
TierLabel.TextSize = 22

-- Gear / Fragments
local GearLabel = Instance.new("TextLabel", ContentFrame)
GearLabel.Size = UDim2.new(1, 0, 0, 26)
GearLabel.BackgroundTransparency = 1
GearLabel.Text = "..."
GearLabel.TextColor3 = Color3.fromRGB(255,255,255)
GearLabel.Font = Enum.Font.GothamBold
GearLabel.TextSize = 22

-- Place
local PlaceLabel = Instance.new("TextLabel", ContentFrame)
PlaceLabel.Size = UDim2.new(1, 0, 0, 22)
PlaceLabel.BackgroundTransparency = 1
PlaceLabel.Text = tostring(game.PlaceId)
PlaceLabel.Font = Enum.Font.GothamBold
PlaceLabel.TextSize = 22

task.spawn(function()
	local player = game.Players.LocalPlayer
	local CommF = game.ReplicatedStorage.Remotes.CommF_

	while task.wait() do
		local v619, v620, v621 = CommF:InvokeServer("UpgradeRace", "Check")

		-- Username
		local name = player.Name
		UsernameLabel.Text = name
		UsernameLabel.TextSize = #name <= 17 and 22 or #name <= 19 and 20 or #name <= 25 and 18 or 16

		-- Status
		if v620 == 10 then
			TrainingLabel.Text = "Done Race V4"
			TrainingLabel.TextColor3 = Color3.fromRGB(0, 255, 255)
		elseif v619 == nil or v619 == 0 then
			TrainingLabel.Text = "Ready For Trial"
			TrainingLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
		elseif v619 == 1 or v619 == 3 or v619 == 6 or v619 == 8 then
			TrainingLabel.Text = "Training Race V4"
			TrainingLabel.TextColor3 = Color3.fromRGB(255, 200, 0)
		else
			TrainingLabel.Text = "Buy Gears"
			TrainingLabel.TextColor3 = Color3.fromRGB(255, 60, 60)
		end

		-- Tier
		TierLabel.Text = (typeof(v620) == "number" and v620 > 0 and v620 <= 10)
			and ("Tiers  ―  V4  :  " .. v620)
			or "Tiers  ―  V4  :  0"

		-- Gear / Fragments
		local gearsText, gearsColor = "...", Color3.fromRGB(255, 255, 255)

		if TrainingLabel.Text == "Done Race V4" and v620 == 10 then
         		gearsText = "Finished Session"
         		gearsColor = Color3.fromRGB(255, 0, 255)

		elseif TrainingLabel.Text == "Ready For Trial" then
        		gearsText = "Completed"
         		gearsColor = Color3.fromRGB(0, 255, 0)

		elseif TrainingLabel.Text == "Training Race V4" then
    		local t = v620 or 0
    		if t <= 1 then
        		gearsText = "Gears  ―  V4  :  0 / 1"
        		gearsColor = Color3.fromRGB(255, 200, 0)
    		elseif t == 2 then
        		gearsText = "Gears  ―  V4  :  0 / 3"
        		gearsColor = Color3.fromRGB(255, 200, 0)
    		elseif t == 3 then
        		gearsText = "Gears  ―  V4  :  1 / 3"
        		gearsColor = Color3.fromRGB(255, 200, 0)
    		elseif t == 4 then
        		gearsText = "Gears  ―  V4  :  2 / 3"
        		gearsColor = Color3.fromRGB(255, 200, 0)
    		elseif t >= 5 and t <= 9 then
        		gearsText = (10 - t) .. " Training Session"
        		gearsColor = Color3.fromRGB(0, 255, 255)
    		end

		elseif TrainingLabel.Text == "Buy Gears" then
    		if typeof(v621) == "number" then
        		gearsText = tostring(v621) .. " Fragments"
			gearsColor = Color3.fromRGB(255, 100, 255)
		end
	end

		GearLabel.Text = gearsText
		GearLabel.TextColor3 = gearsColor

		-- Place color
		if game.PlaceId == 7449423635 then
			PlaceLabel.TextColor3 = Color3.fromRGB(0, 255, 0)
		elseif game.PlaceId == 100117331123089 then
			PlaceLabel.TextColor3 = Color3.fromRGB(255, 60, 60)
		else
			PlaceLabel.TextColor3 = Color3.fromRGB(255, 105, 180)
		end
	end
end)

-- Draggable GUI
local dragging, dragInput, dragStart, startPos
local UserInputService = game:GetService("UserInputService")
MainFrame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
		dragging = true
		dragStart = input.Position
		startPos = MainFrame.Position
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)
MainFrame.InputChanged:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
		dragInput = input
	end
end)
UserInputService.InputChanged:Connect(function(input)
	if dragging and input == dragInput then
		local delta = input.Position - dragStart
		MainFrame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
	end
end)

-- Force On Top nếu bật
if CustomConfig.OnTop == true then
    task.spawn(function()
        local CoreGui = game:GetService("CoreGui")
        local gui = CoreGui:FindFirstChild("CustomGUI_93063325506256")
        if not gui then return end

        gui.ZIndexBehavior = Enum.ZIndexBehavior.Global
        gui.DisplayOrder = 999999

        while task.wait(1) do
            if gui.Parent ~= CoreGui then
                gui.Parent = CoreGui
            end
            gui.DisplayOrder = 999999
        end
    end)
end
