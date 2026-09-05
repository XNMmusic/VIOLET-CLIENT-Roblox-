--// VIOLET CLIENT // Visual GUI
--// LocalScript -> StarterPlayerScripts
--// Только визуальные настройки клиента

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local Lighting = game:GetService("Lighting")
local UserInputService = game:GetService("UserInputService")

local Player = Players.LocalPlayer

--==================================================
-- CONFIG
--==================================================

local CONFIG = {
	Accent = Color3.fromRGB(170, 80, 255),
	AccentDark = Color3.fromRGB(105, 35, 180),
	Background = Color3.fromRGB(12, 9, 19),
	Panel = Color3.fromRGB(19, 14, 29),
	Panel2 = Color3.fromRGB(25, 18, 38),
	Text = Color3.fromRGB(245, 240, 255),
	Muted = Color3.fromRGB(155, 145, 170),
	Stroke = Color3.fromRGB(65, 40, 85),
}

local function tween(obj, time, props, style, direction)
	local info = TweenInfo.new(
		time or 0.2,
		style or Enum.EasingStyle.Quart,
		direction or Enum.EasingDirection.Out
	)
	TweenService:Create(obj, info, props):Play()
end

--==================================================
-- CLEANUP
--==================================================

local old = Player.PlayerGui:FindFirstChild("VioletClient")
if old then
	old:Destroy()
end

--==================================================
-- GUI
--==================================================

local Gui = Instance.new("ScreenGui")
Gui.Name = "VioletClient"
Gui.ResetOnSpawn = false
Gui.IgnoreGuiInset = true
Gui.Parent = Player.PlayerGui

-- Main
local Main = Instance.new("Frame")
Main.Name = "Main"
Main.Size = UDim2.fromOffset(780, 500)
Main.Position = UDim2.new(0.5, -390, 0.5, -250)
Main.BackgroundColor3 = CONFIG.Background
Main.BorderSizePixel = 0
Main.Parent = Gui

local MainCorner = Instance.new("UICorner")
MainCorner.CornerRadius = UDim.new(0, 14)
MainCorner.Parent = Main

local MainStroke = Instance.new("UIStroke")
MainStroke.Color = CONFIG.Stroke
MainStroke.Thickness = 1
MainStroke.Transparency = 0.25
MainStroke.Parent = Main

-- Top gradient
local Gradient = Instance.new("UIGradient")
Gradient.Color = ColorSequence.new({
	ColorSequenceKeypoint.new(0, Color3.fromRGB(35, 18, 52)),
	ColorSequenceKeypoint.new(1, CONFIG.Background),
})
Gradient.Rotation = 90
Gradient.Parent = Main

--==================================================
-- TOP BAR
--==================================================

local Top = Instance.new("Frame")
Top.Size = UDim2.new(1, 0, 0, 68)
Top.BackgroundTransparency = 1
Top.Parent = Main

local Title = Instance.new("TextLabel")
Title.BackgroundTransparency = 1
Title.Position = UDim2.fromOffset(25, 10)
Title.Size = UDim2.fromOffset(300, 30)
Title.Font = Enum.Font.GothamBold
Title.Text = "VIOLET CLIENT"
Title.TextColor3 = CONFIG.Text
Title.TextSize = 21
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = Top

local SubTitle = Instance.new("TextLabel")
SubTitle.BackgroundTransparency = 1
SubTitle.Position = UDim2.fromOffset(27, 38)
SubTitle.Size = UDim2.fromOffset(300, 20)
SubTitle.Font = Enum.Font.Gotham
SubTitle.Text = "VISUAL CONFIGURATION"
SubTitle.TextColor3 = CONFIG.Accent
SubTitle.TextSize = 10
SubTitle.TextXAlignment = Enum.TextXAlignment.Left
SubTitle.Parent = Top

-- Close
local Close = Instance.new("TextButton")
Close.Size = UDim2.fromOffset(38, 38)
Close.Position = UDim2.new(1, -50, 0, 15)
Close.BackgroundColor3 = Color3.fromRGB(35, 20, 43)
Close.Text = "×"
Close.TextColor3 = CONFIG.Muted
Close.TextSize = 25
Close.Font = Enum.Font.GothamBold
Close.AutoButtonColor = false
Close.Parent = Top

local CloseCorner = Instance.new("UICorner")
CloseCorner.CornerRadius = UDim.new(0, 9)
CloseCorner.Parent = Close

Close.MouseEnter:Connect(function()
	tween(Close, 0.15, {
		BackgroundColor3 = Color3.fromRGB(85, 35, 70),
		TextColor3 = Color3.fromRGB(255, 120, 180)
	})
end)

Close.MouseLeave:Connect(function()
	tween(Close, 0.15, {
		BackgroundColor3 = Color3.fromRGB(35, 20, 43),
		TextColor3 = CONFIG.Muted
	})
end)

Close.MouseButton1Click:Connect(function()
	tween(Main, 0.25, {
		Size = UDim2.fromOffset(700, 0)
	})
	task.wait(0.25)
	Main.Visible = false
end)

--==================================================
-- SIDEBAR
--==================================================

local Sidebar = Instance.new("Frame")
Sidebar.Position = UDim2.fromOffset(15, 78)
Sidebar.Size = UDim2.fromOffset(170, 405)
Sidebar.BackgroundColor3 = CONFIG.Panel
Sidebar.BorderSizePixel = 0
Sidebar.Parent = Main

local SidebarCorner = Instance.new("UICorner")
SidebarCorner.CornerRadius = UDim.new(0, 11)
SidebarCorner.Parent = Sidebar

local SidePadding = Instance.new("UIPadding")
SidePadding.PaddingTop = UDim.new(0, 12)
SidePadding.PaddingLeft = UDim.new(0, 10)
SidePadding.PaddingRight = UDim.new(0, 10)
SidePadding.Parent = Sidebar

local SideLayout = Instance.new("UIListLayout")
SideLayout.Padding = UDim.new(0, 6)
SideLayout.Parent = Sidebar

--==================================================
-- CONTENT
--==================================================

local Content = Instance.new("Frame")
Content.Position = UDim2.fromOffset(198, 78)
Content.Size = UDim2.new(1, -213, 0, 405)
Content.BackgroundTransparency = 1
Content.Parent = Main

local Pages = {}

local function createPage(name)
	local page = Instance.new("ScrollingFrame")
	page.Name = name
	page.Size = UDim2.fromScale(1, 1)
	page.BackgroundTransparency = 1
	page.BorderSizePixel = 0
	page.ScrollBarThickness = 3
	page.ScrollBarImageColor3 = CONFIG.Accent
	page.Visible = false
	page.CanvasSize = UDim2.new(0, 0, 0, 0)
	page.AutomaticCanvasSize = Enum.AutomaticSize.Y
	page.Parent = Content

	local layout = Instance.new("UIListLayout")
	layout.Padding = UDim.new(0, 8)
	layout.Parent = page

	local padding = Instance.new("UIPadding")
	padding.PaddingRight = UDim.new(0, 8)
	padding.PaddingBottom = UDim.new(0, 15)
	padding.Parent = page

	Pages[name] = page
	return page
end

local Visuals = createPage("Visuals")
local World = createPage("World")
local Camera = createPage("Camera")
local Performance = createPage("Performance")
local Interface = createPage("Interface")

--==================================================
-- SIDEBAR BUTTON
--==================================================

local CurrentPage

local function selectPage(name)
	for pageName, page in pairs(Pages) do
		page.Visible = pageName == name
	end

	for _, button in ipairs(Sidebar:GetChildren()) do
		if button:IsA("TextButton") then
			if button.Name == name then
				tween(button, 0.18, {
					BackgroundColor3 = CONFIG.AccentDark,
					TextColor3 = CONFIG.Text
				})
			else
				tween(button, 0.18, {
					BackgroundColor3 = Color3.fromRGB(24, 17, 32),
					TextColor3 = CONFIG.Muted
				})
			end
		end
	end

	CurrentPage = name
end

local function createTab(name, icon)
	local button = Instance.new("TextButton")
	button.Name = name
	button.Size = UDim2.new(1, 0, 0, 42)
	button.BackgroundColor3 = Color3.fromRGB(24, 17, 32)
	button.BorderSizePixel = 0
	button.AutoButtonColor = false
	button.Font = Enum.Font.GothamMedium
	button.Text = "   " .. icon .. "   " .. name
	button.TextColor3 = CONFIG.Muted
	button.TextSize = 12
	button.TextXAlignment = Enum.TextXAlignment.Left
	button.Parent = Sidebar

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 8)
	corner.Parent = button

	button.MouseEnter:Connect(function()
		if CurrentPage ~= name then
			tween(button, 0.15, {
				BackgroundColor3 = Color3.fromRGB(32, 23, 43)
			})
		end
	end)

	button.MouseLeave:Connect(function()
		if CurrentPage ~= name then
			tween(button, 0.15, {
				BackgroundColor3 = Color3.fromRGB(24, 17, 32)
			})
		end
	end)

	button.MouseButton1Click:Connect(function()
		selectPage(name)
	end)
end

createTab("Visuals", "✦")
createTab("World", "☁")
createTab("Camera", "◉")
createTab("Performance", "⚡")
createTab("Interface", "⚙")

--==================================================
-- SECTION
--==================================================

local function section(parent, title, description)
	local holder = Instance.new("Frame")
	holder.Size = UDim2.new(1, 0, 0, 48)
	holder.BackgroundTransparency = 1
	holder.Parent = parent

	local titleLabel = Instance.new("TextLabel")
	titleLabel.BackgroundTransparency = 1
	titleLabel.Position = UDim2.fromOffset(4, 0)
	titleLabel.Size = UDim2.new(1, -8, 0, 24)
	titleLabel.Font = Enum.Font.GothamBold
	titleLabel.Text = title
	titleLabel.TextColor3 = CONFIG.Text
	titleLabel.TextSize = 15
	titleLabel.TextXAlignment = Enum.TextXAlignment.Left
	titleLabel.Parent = holder

	local desc = Instance.new("TextLabel")
	desc.BackgroundTransparency = 1
	desc.Position = UDim2.fromOffset(4, 23)
	desc.Size = UDim2.new(1, -8, 0, 20)
	desc.Font = Enum.Font.Gotham
	desc.Text = description or ""
	desc.TextColor3 = CONFIG.Muted
	desc.TextSize = 10
	desc.TextXAlignment = Enum.TextXAlignment.Left
	desc.Parent = holder
end

--==================================================
-- TOGGLE
--==================================================

local function toggle(parent, title, description, default, callback)
	local frame = Instance.new("Frame")
	frame.Size = UDim2.new(1, 0, 0, 55)
	frame.BackgroundColor3 = CONFIG.Panel2
	frame.BorderSizePixel = 0
	frame.Parent = parent

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 9)
	corner.Parent = frame

	local label = Instance.new("TextLabel")
	label.BackgroundTransparency = 1
	label.Position = UDim2.fromOffset(14, 7)
	label.Size = UDim2.new(1, -80, 0, 20)
	label.Font = Enum.Font.GothamMedium
	label.Text = title
	label.TextColor3 = CONFIG.Text
	label.TextSize = 12
	label.TextXAlignment = Enum.TextXAlignment.Left
	label.Parent = frame

	local desc = Instance.new("TextLabel")
	desc.BackgroundTransparency = 1
	desc.Position = UDim2.fromOffset(14, 28)
	desc.Size = UDim2.new(1, -80, 0, 18)
	desc.Font = Enum.Font.Gotham
	desc.Text = description or ""
	desc.TextColor3 = CONFIG.Muted
	desc.TextSize = 9
	desc.TextXAlignment = Enum.TextXAlignment.Left
	desc.Parent = frame

	local button = Instance.new("TextButton")
	button.Size = UDim2.fromOffset(44, 24)
	button.Position = UDim2.new(1, -58, 0.5, -12)
	button.BackgroundColor3 = Color3.fromRGB(40, 32, 45)
	button.Text = ""
	button.AutoButtonColor = false
	button.Parent = frame

	local bc = Instance.new("UICorner")
	bc.CornerRadius = UDim.new(1, 0)
	bc.Parent = button

	local knob = Instance.new("Frame")
	knob.Size = UDim2.fromOffset(18, 18)
	knob.Position = UDim2.new(0, 3, 0.5, -9)
	knob.BackgroundColor3 = CONFIG.Muted
	knob.BorderSizePixel = 0
	knob.Parent = button

	local kc = Instance.new("UICorner")
	kc.CornerRadius = UDim.new(1, 0)
	kc.Parent = knob

	local state = default or false

	local function update()
		if state then
			tween(button, 0.18, {
				BackgroundColor3 = CONFIG.AccentDark
			})
			tween(knob, 0.18, {
				Position = UDim2.new(1, -21, 0.5, -9),
				BackgroundColor3 = CONFIG.Text
			})
		else
			tween(button, 0.18, {
				BackgroundColor3 = Color3.fromRGB(40, 32, 45)
			})
			tween(knob, 0.18, {
				Position = UDim2.new(0, 3, 0.5, -9),
				BackgroundColor3 = CONFIG.Muted
			})
		end

		if callback then
			callback(state)
		end
	end

	button.MouseButton1Click:Connect(function()
		state = not state
		update()
	end)

	update()

	return function(value)
		state = value
		update()
	end
end

--==================================================
-- SLIDER
--==================================================

local function slider(parent, title, min, max, default, callback)
	local frame = Instance.new("Frame")
	frame.Size = UDim2.new(1, 0, 0, 65)
	frame.BackgroundColor3 = CONFIG.Panel2
	frame.BorderSizePixel = 0
	frame.Parent = parent

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 9)
	corner.Parent = frame

	local label = Instance.new("TextLabel")
	label.BackgroundTransparency = 1
	label.Position = UDim2.fromOffset(14, 8)
	label.Size = UDim2.new(1, -90, 0, 20)
	label.Font = Enum.Font.GothamMedium
	label.Text = title
	label.TextColor3 = CONFIG.Text
	label.TextSize = 12
	label.TextXAlignment = Enum.TextXAlignment.Left
	label.Parent = frame

	local valueLabel = Instance.new("TextLabel")
	valueLabel.BackgroundTransparency = 1
	valueLabel.Position = UDim2.new(1, -70, 0, 8)
	valueLabel.Size = UDim2.fromOffset(55, 20)
	valueLabel.Font = Enum.Font.GothamBold
	valueLabel.TextColor3 = CONFIG.Accent
	valueLabel.TextSize = 11
	valueLabel.TextXAlignment = Enum.TextXAlignment.Right
	valueLabel.Parent = frame

	local bar = Instance.new("Frame")
	bar.Position = UDim2.fromOffset(14, 40)
	bar.Size = UDim2.new(1, -28, 0, 6)
	bar.BackgroundColor3 = Color3.fromRGB(45, 35, 50)
	bar.BorderSizePixel = 0
	bar.Parent = frame

	local barCorner = Instance.new("UICorner")
	barCorner.CornerRadius = UDim.new(1, 0)
	barCorner.Parent = bar

	local fill = Instance.new("Frame")
	fill.Size = UDim2.fromScale((default - min) / (max - min), 1)
	fill.BackgroundColor3 = CONFIG.Accent
	fill.BorderSizePixel = 0
	fill.Parent = bar

	local fillCorner = Instance.new("UICorner")
	fillCorner.CornerRadius = UDim.new(1, 0)
	fillCorner.Parent = fill

	local knob = Instance.new("Frame")
	knob.Size = UDim2.fromOffset(14, 14)
	knob.AnchorPoint = Vector2.new(0.5, 0.5)
	knob.Position = UDim2.new((default - min) / (max - min), 0, 0.5, 0)
	knob.BackgroundColor3 = CONFIG.Text
	knob.BorderSizePixel = 0
	knob.Parent = bar

	local kc = Instance.new("UICorner")
	kc.CornerRadius = UDim.new(1, 0)
	kc.Parent = knob

	local dragging = false

	local function setValue(value)
		value = math.clamp(value, min, max)
		value = math.floor(value * 10 + 0.5) / 10

		local percent = (value - min) / (max - min)

		fill.Size = UDim2.fromScale(percent, 1)
		knob.Position = UDim2.new(percent, 0, 0.5, 0)
		valueLabel.Text = tostring(value)

		if callback then
			callback(value)
		end
	end

	local function updateFromMouse(x)
		local percent = math.clamp(
			(x - bar.AbsolutePosition.X) / bar.AbsoluteSize.X,
			0,
			1
		)

		setValue(min + (max - min) * percent)
	end

	bar.InputBegan:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1
			or input.UserInputType == Enum.UserInputType.Touch then

			dragging = true
			updateFromMouse(input.Position.X)
		end
	end)

	UserInputService.InputChanged:Connect(function(input)
		if dragging and (
			input.UserInputType == Enum.UserInputType.MouseMovement
			or input.UserInputType == Enum.UserInputType.Touch
		) then
			updateFromMouse(input.Position.X)
		end
	end)

	UserInputService.InputEnded:Connect(function(input)
		if input.UserInputType == Enum.UserInputType.MouseButton1
			or input.UserInputType == Enum.UserInputType.Touch then
			dragging = false
		end
	end)

	setValue(default)
end

--==================================================
-- DROPDOWN
--==================================================

local function dropdown(parent, title, options, default, callback)
	local frame = Instance.new("Frame")
	frame.Size = UDim2.new(1, 0, 0, 55)
	frame.BackgroundColor3 = CONFIG.Panel2
	frame.BorderSizePixel = 0
	frame.ClipsDescendants = true
	frame.Parent = parent

	local corner = Instance.new("UICorner")
	corner.CornerRadius = UDim.new(0, 9)
	corner.Parent = frame

	local button = Instance.new("TextButton")
	button.Size = UDim2.new(1, -20, 0, 55)
	button.Position = UDim2.fromOffset(10, 0)
	button.BackgroundTransparency = 1
	button.Text = ""
	button.Parent = frame

	local titleLabel = Instance.new("TextLabel")
	titleLabel.BackgroundTransparency = 1
	titleLabel.Position = UDim2.fromOffset(5, 6)
	titleLabel.Size = UDim2.new(0.5, 0, 0, 20)
	titleLabel.Font = Enum.Font.GothamMedium
	titleLabel.Text = title
	titleLabel.TextColor3 = CONFIG.Text
	titleLabel.TextSize = 12
	titleLabel.TextXAlignment = Enum.TextXAlignment.Left
	titleLabel.Parent = button

	local selected = Instance.new("TextLabel")
	selected.BackgroundTransparency = 1
	selected.Position = UDim2.new(0.5, 0, 0, 6)
	selected.Size = UDim2.new(0.5, -25, 0, 20)
	selected.Font = Enum.Font.GothamMedium
	selected.Text = default
	selected.TextColor3 = CONFIG.Accent
	selected.TextSize = 11
	selected.TextXAlignment = Enum.TextXAlignment.Right
	selected.Parent = button

	local arrow = Instance.new("TextLabel")
	arrow.BackgroundTransparency = 1
	arrow.Position = UDim2.new(1, -25, 0, 6)
	arrow.Size = UDim2.fromOffset(20, 20)
	arrow.Text = "⌄"
	arrow.TextColor3 = CONFIG.Muted
	arrow.TextSize = 15
	arrow.Font = Enum.Font.GothamBold
	arrow.Parent = button

	local opened = false

	local function openMenu()
		opened = not opened

		if opened then
			local extraHeight = #options * 30 + 55
			tween(frame, 0.2, {
				Size = UDim2.new(1, 0, 0, extraHeight)
			})
			arrow.Text = "⌃"
		else
			tween(frame, 0.2, {
				Size = UDim2.new(1, 0, 0, 55)
			})
			arrow.Text = "⌄"
		end
	end

	button.MouseButton1Click:Connect(openMenu)

	for i, option in ipairs(options) do
		local optionButton = Instance.new("TextButton")
		optionButton.Size = UDim2.new(1, -20, 0, 28)
		optionButton.Position = UDim2.fromOffset(10, 55 + (i - 1) * 30)
		optionButton.BackgroundColor3 = Color3.fromRGB(30, 21, 38)
		optionButton.BorderSizePixel = 0
		optionButton.Text = option
		optionButton.TextColor3 = CONFIG.Muted
		optionButton.TextSize = 10
		optionButton.Font = Enum.Font.Gotham
		optionButton.AutoButtonColor = false
		optionButton.Parent = frame

		local oc = Instance.new("UICorner")
		oc.CornerRadius = UDim.new(0, 6)
		oc.Parent = optionButton

		optionButton.MouseEnter:Connect(function()
			tween(optionButton, 0.12, {
				BackgroundColor3 = CONFIG.AccentDark,
				TextColor3 = CONFIG.Text
			})
		end)

		optionButton.MouseLeave:Connect(function()
			tween(optionButton, 0.12, {
				BackgroundColor3 = Color3.fromRGB(30, 21, 38),
				TextColor3 = CONFIG.Muted
			})
		end)

		optionButton.MouseButton1Click:Connect(function()
			selected.Text = option
			opened = false

			tween(frame, 0.2, {
				Size = UDim2.new(1, 0, 0, 55)
			})

			arrow.Text = "⌄"

			if callback then
				callback(option)
			end
		end)
	end

	if callback then
		callback(default)
	end
end

--==================================================
-- LIGHTING
--==================================================

local ColorCorrection = Lighting:FindFirstChild("VioletColorCorrection")

if not ColorCorrection then
	ColorCorrection = Instance.new("ColorCorrectionEffect")
	ColorCorrection.Name = "VioletColorCorrection"
	ColorCorrection.Parent = Lighting
end

local Bloom = Lighting:FindFirstChild("VioletBloom")

if not Bloom then
	Bloom = Instance.new("BloomEffect")
	Bloom.Name = "VioletBloom"
	Bloom.Parent = Lighting
end

local SunRays = Lighting:FindFirstChild("VioletSunRays")

if not SunRays then
	SunRays = Instance.new("SunRaysEffect")
	SunRays.Name = "VioletSunRays"
	SunRays.Parent = Lighting
end

local Atmosphere = Lighting:FindFirstChild("VioletAtmosphere")

if not Atmosphere then
	Atmosphere = Instance.new("Atmosphere")
	Atmosphere.Name = "VioletAtmosphere"
	Atmosphere.Parent = Lighting
end

--==================================================
-- VISUALS PAGE
--==================================================

section(
	Visuals,
	"Visual Effects",
	"Настройки графики и пост-эффектов"
)

toggle(
	Visuals,
	"Bloom",
	"Мягкое свечение ярких объектов",
	true,
	function(state)
		Bloom.Enabled = state
	end
)

slider(
	Visuals,
	"Bloom Intensity",
	0,
	3,
	0.8,
	function(value)
		Bloom.Intensity = value
	end
)

slider(
	Visuals,
	"Brightness",
	-2,
	2,
	0.2,
	function(value)
		ColorCorrection.Brightness = value
	end
)

slider(
	Visuals,
	"Contrast",
	-1,
	1,
	0.15,
	function(value)
		ColorCorrection.Contrast = value
	end
)

slider(
	Visuals,
	"Saturation",
	-1,
	1,
	0.2,
	function(value)
		ColorCorrection.Saturation = value
	end
)

toggle(
	Visuals,
	"Sun Rays",
	"Солнечные лучи и атмосферное свечение",
	true,
	function(state)
		SunRays.Enabled = state
	end
)

slider(
	Visuals,
	"Sun Ray Intensity",
	0,
	1,
	0.08,
	function(value)
		SunRays.Intensity = value
	end
)

--==================================================
-- WORLD PAGE
--==================================================

section(
	World,
	"World",
	"Атмосфера и варианты неба"
)

local function setSky(style)
	if style == "Violet" then
		Atmosphere.Color = Color3.fromRGB(170, 100, 255)
		Atmosphere.Decay = Color3.fromRGB(55, 30, 85)
		Atmosphere.Density = 0.25
		Atmosphere.Glare = 0.15
		Atmosphere.Haze = 1

	elseif style == "Midnight" then
		Atmosphere.Color = Color3.fromRGB(70, 90, 180)
		Atmosphere.Decay = Color3.fromRGB(10, 15, 50)
		Atmosphere.Density = 0.32
		Atmosphere.Glare = 0.05
		Atmosphere.Haze = 1.5

	elseif style == "Sunset" then
		Atmosphere.Color = Color3.fromRGB(255, 145, 90)
		Atmosphere.Decay = Color3.fromRGB(100, 45, 35)
		Atmosphere.Density = 0.2
		Atmosphere.Glare = 0.3
		Atmosphere.Haze = 1.2

	elseif style == "Cyber" then
		Atmosphere.Color = Color3.fromRGB(90, 255, 245)
		Atmosphere.Decay = Color3.fromRGB(65, 15, 100)
		Atmosphere.Density = 0.18
		Atmosphere.Glare = 0.4
		Atmosphere.Haze = 0.7

	elseif style == "Foggy" then
		Atmosphere.Color = Color3.fromRGB(180, 180, 190)
		Atmosphere.Decay = Color3.fromRGB(65, 65, 75)
		Atmosphere.Density = 0.5
		Atmosphere.Glare = 0
		Atmosphere.Haze = 3
	end
end

dropdown(
	World,
	"Sky Style",
	{
		"Violet",
		"Midnight",
		"Sunset",
		"Cyber",
		"Foggy"
	},
	"Violet",
	function(option)
		setSky(option)
	end
)

toggle(
	World,
	"Atmosphere",
	"Объёмная атмосфера мира",
	true,
	function(state)
		Atmosphere.Enabled = state
	end
)

slider(
	World,
	"Fog Density",
	0,
	1,
	0.25,
	function(value)
		Atmosphere.Density = value
	end
)

slider(
	World,
	"Haze",
	0,
	5,
	1,
	function(value)
		Atmosphere.Haze = value
	end
)

slider(
	World,
	"Exposure",
	-3,
	3,
	0,
	function(value)
		Lighting.ExposureCompensation = value
	end
)

--==================================================
-- CAMERA PAGE
--==================================================

section(
	Camera,
	"Camera",
	"Настройки визуального поведения камеры"
)

local CameraObject = workspace.CurrentCamera

slider(
	Camera,
	"Field Of View",
	50,
	120,
	70,
	function(value)
		CameraObject.FieldOfView = value
	end
)

toggle(
	Camera,
	"Camera Bob",
	"Лёгкое покачивание камеры",
	false,
	function(state)
		-- Включение визуального эффекта можно добавить позже.
	end
)

toggle(
	Camera,
	"Dynamic FOV",
	"Плавное изменение FOV",
	false,
	function(state)
		-- Зарезервировано под динамический FOV.
	end
)

toggle(
	Camera,
	"Smooth Camera",
	"Более плавное ощущение движения камеры",
	true,
	function(state)
		-- Только визуальный переключатель.
	end
)

--==================================================
-- PERFORMANCE PAGE
--==================================================

section(
	Performance,
	"Performance",
	"Оптимизация локальных визуальных эффектов"
)

local PerformanceMode = false

toggle(
	Performance,
	"FPS Boost",
	"Уменьшает локальные графические эффекты",
	false,
	function(state)
		PerformanceMode = state

		if state then
			Bloom.Enabled = false
			SunRays.Enabled = false

			Atmosphere.Density = 0.05
			Atmosphere.Haze = 0

			ColorCorrection.Contrast = 0
			ColorCorrection.Saturation = 0
		else
			Bloom.Enabled = true
			SunRays.Enabled = true

			Atmosphere.Density = 0.25
			Atmosphere.Haze = 1

			ColorCorrection.Contrast = 0.15
			ColorCorrection.Saturation = 0.2
		end
	end
)

toggle(
	Performance,
	"Disable Bloom",
	"Отключить свечение для повышения производительности",
	false,
	function(state)
		Bloom.Enabled = not state
	end
)

toggle(
	Performance,
	"Disable Sun Rays",
	"Отключить солнечные лучи",
	false,
	function(state)
		SunRays.Enabled = not state
	end
)

toggle(
	Performance,
	"Low Atmosphere",
	"Уменьшить нагрузку от атмосферы",
	false,
	function(state)
		if state then
			Atmosphere.Density = 0.05
			Atmosphere.Haze = 0
		else
			Atmosphere.Density = 0.25
			Atmosphere.Haze = 1
		end
	end
)

--==================================================
-- INTERFACE PAGE
--==================================================

section(
	Interface,
	"Interface",
	"Внешний вид самого Violet Client"
)

slider(
	Interface,
	"UI Transparency",
	0,
	0.6,
	0,
	function(value)
		Main.BackgroundTransparency = value
	end
)

slider(
	Interface,
	"UI Scale",
	0.8,
	1.2,
	1,
	function(value)
		Main.Size = UDim2.fromOffset(
			math.floor(780 * value),
			math.floor(500 * value)
		)
	end
)

toggle(
	Interface,
	"Animated UI",
	"Плавные анимации интерфейса",
	true,
	function(state)
		-- Используется системой интерфейса.
	end
)

toggle(
	Interface,
	"Neon Accent",
	"Более яркий фиолетовый акцент",
	true,
	function(state)
		if state then
			CONFIG.Accent = Color3.fromRGB(190, 80, 255)
		else
			CONFIG.Accent = Color3.fromRGB(140, 70, 200)
		end
	end
)

--==================================================
-- OPEN BUTTON
--==================================================

local OpenButton = Instance.new("TextButton")
OpenButton.Name = "OpenButton"
OpenButton.Size = UDim2.fromOffset(48, 48)
OpenButton.Position = UDim2.new(0, 20, 0.5, -24)
OpenButton.BackgroundColor3 = CONFIG.AccentDark
OpenButton.Text = "V"
OpenButton.TextColor3 = CONFIG.Text
OpenButton.TextSize = 20
OpenButton.Font = Enum.Font.GothamBold
OpenButton.AutoButtonColor = false
OpenButton.Visible = false
OpenButton.Parent = Gui

local OpenCorner = Instance.new("UICorner")
OpenCorner.CornerRadius = UDim.new(0, 12)
OpenCorner.Parent = OpenButton

OpenButton.MouseEnter:Connect(function()
	tween(OpenButton, 0.15, {
		Size = UDim2.fromOffset(54, 54)
	})
end)

OpenButton.MouseLeave:Connect(function()
	tween(OpenButton, 0.15, {
		Size = UDim2.fromOffset(48, 48)
	})
end)

OpenButton.MouseButton1Click:Connect(function()
	OpenButton.Visible = false
	Main.Visible = true
	Main.Size = UDim2.fromOffset(0, 0)

	tween(Main, 0.3, {
		Size = UDim2.fromOffset(780, 500)
	})
end)

--==================================================
-- CLOSE -> OPEN
--==================================================

Close.MouseButton1Click:Connect(function()
	tween(Main, 0.25, {
		Size = UDim2.fromOffset(0, 0)
	})

	task.wait(0.25)

	Main.Visible = false
	Main.Size = UDim2.fromOffset(780, 500)
	OpenButton.Visible = true
end)

--==================================================
-- DRAGGING
--==================================================

local dragging = false
local dragStart
local startPos

Top.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
		or input.UserInputType == Enum.UserInputType.Touch then

		dragging = true
		dragStart = input.Position
		startPos = Main.Position
	end
end)

Top.InputEnded:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1
		or input.UserInputType == Enum.UserInputType.Touch then

		dragging = false
	end
end)

UserInputService.InputChanged:Connect(function(input)
	if dragging and (
		input.UserInputType == Enum.UserInputType.MouseMovement
		or input.UserInputType == Enum.UserInputType.Touch
	) then

		local delta = input.Position - dragStart

		Main.Position = UDim2.new(
			startPos.X.Scale,
			startPos.X.Offset + delta.X,
			startPos.Y.Scale,
			startPos.Y.Offset + delta.Y
		)
	end
end)

--==================================================
-- KEYBIND
-- RightShift opens/closes GUI
--==================================================

UserInputService.InputBegan:Connect(function(input, processed)
	if processed then
		return
	end

	if input.KeyCode == Enum.KeyCode.RightShift then
		if Main.Visible then
			tween(Main, 0.22, {
				Size = UDim2.fromOffset(0, 0)
			})

			task.wait(0.22)

			Main.Visible = false
			Main.Size = UDim2.fromOffset(780, 500)
			OpenButton.Visible = true
		else
			OpenButton.Visible = false
			Main.Visible = true
			Main.Size = UDim2.fromOffset(0, 0)

			tween(Main, 0.3, {
				Size = UDim2.fromOffset(780, 500)
			})
		end
	end
end)

--==================================================
-- START
--==================================================

selectPage("Visuals")
setSky("Violet")

print("Violet Client loaded successfully.")
print("Press RightShift to open/close the GUI.")
