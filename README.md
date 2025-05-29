local TweenService = game:GetService("TweenService")
local gui = script.Parent

local devIconsFolder = gui:WaitForChild("devicons")
local devTagsFolder = gui:WaitForChild("dev tags")
local quoteLabel = gui:WaitForChild("quote")
local sideLeft = gui:WaitForChild("side left")
local sideRight = gui:WaitForChild("side right")

-- Get dev icons (names "1" to "6")
local devIcons = {}
for i = 1, 6 do
    table.insert(devIcons, devIconsFolder:FindFirstChild(tostring(i)))
end

-- Sort dev tags (optional: sort by Name)
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

    -- Start offscreen: top (odd) or bottom (even), keep X the same
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

-- Ensure split screen frames are visible and pre-positioned in Studio
-- Do not change size or position here

-- ========== TIMELINE ==========

local slideInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
local fadeInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad)

-- 1. Slide in icons (one at a time)
for i, icon in ipairs(devIcons) do
    TweenService:Create(icon, slideInfo, {Position = finalPositions[i]}):Play()
    task.wait(0.2)
end

-- 2. Fade in dev name tags
for _, tag in ipairs(devTags) do
    TweenService:Create(tag, fadeInfo, {TextTransparency = 0}):Play()
end

-- 3. Show quote
TweenService:Create(quoteLabel, fadeInfo, {TextTransparency = 0}):Play()

-- 4. Wait
task.wait(2)

-- 5. Fade out all dev UI
for _, icon in ipairs(devIcons) do
    TweenService:Create(icon, fadeInfo, {ImageTransparency = 1}):Play()
end
for _, tag in ipairs(devTags) do
    TweenService:Create(tag, fadeInfo, {TextTransparency = 1}):Play()
end
TweenService:Create(quoteLabel, fadeInfo, {TextTransparency = 1}):Play()

-- 6. Diagonal split screen (frames already rotated/positioned)
TweenService:Create(sideLeft, TweenInfo.new(0.6, Enum.EasingStyle.Sine), {
    Position = sideLeft.Position + UDim2.new(-1, 0, 1, 0)
}):Play()

TweenService:Create(sideRight, TweenInfo.new(0.6, Enum.EasingStyle.Sine), {
    Position = sideRight.Position + UDim2.new(1, 0, -1, 0)
}):Play()

-- Optional: wait and then show main menu
-- task.wait(0.6)
-- gui:WaitForChild("MainMenu").Visible = true
