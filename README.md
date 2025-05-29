local TweenService = game:GetService("TweenService")
local gui = script.Parent

local devIconsFolder = gui:WaitForChild("devicons")
local devTagsFolder = gui:WaitForChild("dev tags")
local quoteLabel = gui:WaitForChild("quote")
local sideLeft = gui:WaitForChild("side left")
local sideRight = gui:WaitForChild("side right")

-- Get and sort dev icons (by name 1–6)
local devIcons = {}
for i = 1, 6 do
    table.insert(devIcons, devIconsFolder:FindFirstChild(tostring(i)))
end

-- Get and sort dev tags
local devTags = devTagsFolder:GetChildren()
table.sort(devTags, function(a, b) return a.Name < b.Name end)

-- Quotes
local quotes = {
    "Great things take time.",
    "Code, test, repeat.",
    "Created with passion.",
    "From bugs to brilliance.",
    "Stay inspired.",
    "Built by dreamers.",
    "Powered by teamwork."
}

-- ========== SETUP ==========

-- Store original icon positions
local finalPositions = {}

for i, icon in ipairs(devIcons) do
    icon.Visible = true
    icon.ImageTransparency = 0

    finalPositions[i] = icon.Position

    -- Slide in from top (odd) or bottom (even)
    local offscreenY = (i % 2 == 1) and UDim.new(-1, 0) or UDim.new(2, 0)
    icon.Position = UDim2.new(finalPositions[i].X, finalPositions[i].X.Offset, offscreenY, 0)
end

-- Hide dev tags
for _, tag in ipairs(devTags) do
    tag.Visible = true
    tag.TextTransparency = 1
end

-- Setup quote
quoteLabel.Visible = true
quoteLabel.TextTransparency = 1
quoteLabel.Text = quotes[math.random(1, #quotes)]

-- Setup split frames (make sure these are already visible in Studio)
sideLeft.Size = UDim2.new(0, 0, 1, 0)
sideLeft.Position = UDim2.new(0, 0, 0, 0)

sideRight.Size = UDim2.new(0, 0, 1, 0)
sideRight.Position = UDim2.new(1, 0, 0, 0)

-- ========== TIMELINE ==========

local slideInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
local fadeInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad)

-- 1. Slide in icons individually
for i, icon in ipairs(devIcons) do
    TweenService:Create(icon, slideInfo, {Position = finalPositions[i]}):Play()
    task.wait(0.2)
end

-- 2. Fade in developer name tags
for _, tag in ipairs(devTags) do
    TweenService:Create(tag, fadeInfo, {TextTransparency = 0}):Play()
end

-- 3. Show quote
TweenService:Create(quoteLabel, fadeInfo, {TextTransparency = 0}):Play()

-- 4. Wait
task.wait(2)

-- 5. Fade out all dev elements
for _, icon in ipairs(devIcons) do
    TweenService:Create(icon, fadeInfo, {ImageTransparency = 1}):Play()
end
for _, tag in ipairs(devTags) do
    TweenService:Create(tag, fadeInfo, {TextTransparency = 1}):Play()
end
TweenService:Create(quoteLabel, fadeInfo, {TextTransparency = 1}):Play()

-- 6. Split screen animation
TweenService:Create(sideLeft, TweenInfo.new(0.6, Enum.EasingStyle.Sine), {
    Size = UDim2.new(0.5, 0, 1, 0)
}):Play()

TweenService:Create(sideRight, TweenInfo.new(0.6, Enum.EasingStyle.Sine), {
    Size = UDim2.new(0.5, 0, 1, 0),
    Position = UDim2.new(0.5, 0, 0, 0)
}):Play()

-- Optional: wait and then show your main menu here
-- task.wait(0.6)
-- gui:WaitForChild("MainMenu").Visible = true
