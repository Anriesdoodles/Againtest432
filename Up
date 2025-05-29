local TweenService = game:GetService("TweenService")
local gui = script.Parent

local devIconsFolder = gui:WaitForChild("devicons")
local devTagsFolder = gui:WaitForChild("dev tags")
local quoteLabel = gui:WaitForChild("quote")
local sideLeft = gui:WaitForChild("side left")
local sideRight = gui:WaitForChild("side right")

-- Get dev icons (named 1 to 6)
local devIcons = {}
for i = 1, 6 do
    table.insert(devIcons, devIconsFolder:FindFirstChild(tostring(i)))
end

-- Sort dev tags
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

-- Short delay before anything starts
task.wait(0.5)

-- ========== SETUP ==========

-- Store final positions
local finalPositions = {}

-- Get screen size (to calculate offscreen)
local viewportSize = gui.AbsoluteSize

for i, icon in ipairs(devIcons) do
    icon.Visible = true
    icon.ImageTransparency = 0

    -- Save their final position
    finalPositions[i] = icon.Position

    -- Move offscreen vertically (top or bottom)
    local offsetY
    if i % 2 == 1 then
        offsetY = -icon.AbsoluteSize.Y - 50 -- slide from top
    else
        offsetY = viewportSize.Y + icon.AbsoluteSize.Y + 50 -- slide from bottom
    end

    -- Apply new starting Position offscreen
    icon.Position = UDim2.new(
        icon.Position.X.Scale,
        icon.Position.X.Offset,
        0,
        offsetY
    )
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

-- Ensure split screen frames are already rotated & visible in Studio

-- ========== TIMELINE ==========

local slideInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
local fadeInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad)

-- 1. Slide in icons
for i, icon in ipairs(devIcons) do
    TweenService:Create(icon, slideInfo, {Position = finalPositions[i]}):Play()
    task.wait(0.2)
end

-- 2. Fade in dev tags
for _, tag in ipairs(devTags) do
    TweenService:Create(tag, fadeInfo, {TextTransparency = 0}):Play()
end

-- 3. Show quote
TweenService:Create(quoteLabel, fadeInfo, {TextTransparency = 0}):Play()

-- 4. Wait
task.wait(2)

-- 5. Fade out icons and tags
for _, icon in ipairs(devIcons) do
    TweenService:Create(icon, fadeInfo, {ImageTransparency = 1}):Play()
end
for _, tag in ipairs(devTags) do
    TweenService:Create(tag, fadeInfo, {TextTransparency = 1}):Play()
end
TweenService:Create(quoteLabel, fadeInfo, {TextTransparency = 1}):Play()

-- 6. Diagonal split animation
TweenService:Create(sideLeft, TweenInfo.new(0.6, Enum.EasingStyle.Sine), {
    Position = sideLeft.Position + UDim2.new(-1, 0, 1, 0)
}):Play()

TweenService:Create(sideRight, TweenInfo.new(0.6, Enum.EasingStyle.Sine), {
    Position = sideRight.Position + UDim2.new(1, 0, -1, 0)
}):Play()
