local TweenService = game:GetService("TweenService")
local gui = script.Parent

local devIconsFolder = gui:WaitForChild("devicons")
local devTagsFolder = gui:WaitForChild("dev tags")
local quoteLabel = gui:WaitForChild("quote")
local sideLeft = gui:WaitForChild("side left")
local sideRight = gui:WaitForChild("side right")

local devIcons = {} -- Sorted numerically
for i = 1, 6 do
    table.insert(devIcons, devIconsFolder:FindFirstChild(tostring(i)))
end

local devTags = devTagsFolder:GetChildren()
table.sort(devTags, function(a, b) return a.Name < b.Name end)

local quotes = {
    "Great things take time.",
    "Code, test, repeat.",
    "Created with passion.",
    "From bugs to brilliance.",
    "Stay inspired.",
    "Built by dreamers.",
    "Powered by teamwork."
}

-- Set starting properties
for i, icon in ipairs(devIcons) do
    icon.Visible = true
    icon.ImageTransparency = 0
    icon.Position = UDim2.new(0.15 + (i-1)*0.12, 0, (i % 2 == 1) and -1 or 2, 0)
end

for _, tag in ipairs(devTags) do
    tag.Visible = true
    tag.TextTransparency = 1
end

quoteLabel.Visible = true
quoteLabel.TextTransparency = 1
quoteLabel.Text = quotes[math.random(1, #quotes)]

sideLeft.Size = UDim2.new(0, 0, 1, 0)
sideRight.Size = UDim2.new(0, 0, 1, 0)
sideRight.Position = UDim2.new(1, 0, 0, 0)

-- Tween settings
local slideInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
local fadeInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad)

-- Step 1: Slide icons in individually (top/bottom)
for i, icon in ipairs(devIcons) do
    local goalPos = UDim2.new(0.15 + (i-1)*0.12, 0, 0.3, 0)
    TweenService:Create(icon, slideInfo, {Position = goalPos}):Play()
    task.wait(0.2)
end

-- Step 2: Fade in dev tags
for _, tag in ipairs(devTags) do
    TweenService:Create(tag, fadeInfo, {TextTransparency = 0}):Play()
end

-- Step 3: Show random quote
TweenService:Create(quoteLabel, fadeInfo, {TextTransparency = 0}):Play()

-- Step 4: Wait
task.wait(2)

-- Step 5: Fade out all elements
for _, icon in ipairs(devIcons) do
    TweenService:Create(icon, fadeInfo, {ImageTransparency = 1}):Play()
end
for _, tag in ipairs(devTags) do
    TweenService:Create(tag, fadeInfo, {TextTransparency = 1}):Play()
end
TweenService:Create(quoteLabel, fadeInfo, {TextTransparency = 1}):Play()

-- Step 6: Split screen animation
TweenService:Create(sideLeft, TweenInfo.new(0.6, Enum.EasingStyle.Sine), {
    Size = UDim2.new(0.5, 0, 1, 0)
}):Play()

TweenService:Create(sideRight, TweenInfo.new(0.6, Enum.EasingStyle.Sine), {
    Size = UDim2.new(0.5, 0, 1, 0),
    Position = UDim2.new(0.5, 0, 0, 0)
}):Play()

-- Optional: Add your menu show logic after a short wait
task.wait(0.6)
-- gui:WaitForChild("MainMenu").Visible = true
