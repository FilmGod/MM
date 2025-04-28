local player = game.Players.LocalPlayer
local screenGui = Instance.new("ScreenGui")
screenGui.Parent = player:WaitForChild("PlayerGui")

-- สร้าง Frame สำหรับ GUI (สามารถลากไปมาได้)
local frame = Instance.new("Frame")
frame.Parent = screenGui
frame.Size = UDim2.new(0.15, 0, 0.25, 0)  -- ขนาดของ Frame ลดลง 2 เท่า
frame.Position = UDim2.new(0.425, 0, 0.375, 0)  -- ปรับตำแหน่งให้เหมาะสม
frame.BackgroundColor3 = Color3.fromRGB(40, 40, 40)  -- สีพื้นหลังแบบทันสมัย
frame.BorderSizePixel = 2
frame.BorderColor3 = Color3.fromRGB(255, 255, 255)
frame.BackgroundTransparency = 0.2  -- ทำให้พื้นหลังโปร่งใสเล็กน้อย

-- ทำให้ GUI สามารถลากไปมาได้
local dragging = false
local dragStart, startPos

frame.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end

    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = frame.Position
    end
end)

frame.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - dragStart
        frame.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

frame.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

-- สร้าง Title
local title = Instance.new("TextLabel")
title.Parent = frame
title.Size = UDim2.new(1, 0, 0.1, 0)
title.Text = "Select Class"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 12  -- ขนาดข้อความเล็กลง 2 เท่า
title.BackgroundTransparency = 1

-- สร้างปุ่มสำหรับเลือกคลาส
local classNames = {
    "Assassin", "Atomic Samurai", "Viking", "Vampire", "Marauder",
    "Knight", "Kaiju", "Guardian", "Fighter", "Dark Mage", "Berserker"
}

-- สร้างปุ่มตามจำนวนคลาส
local buttonHeight = 0.06  -- ปรับความสูงของปุ่มให้เล็กลง 2 เท่า
for i, className in ipairs(classNames) do
    local button = Instance.new("TextButton")
    button.Parent = frame
    button.Size = UDim2.new(1, 0, buttonHeight, 0)
    button.Position = UDim2.new(0, 0, buttonHeight * (i - 1), 0)
    button.Text = className
    button.TextColor3 = Color3.fromRGB(255, 255, 255)
    button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    button.TextSize = 12  -- ขนาดข้อความของปุ่มเล็กลง 2 เท่า
    button.BorderSizePixel = 0
    button.BackgroundTransparency = 0.3
    button.MouseButton1Click:Connect(function()
        -- เมื่อคลิกปุ่ม จะส่งคำสั่งไปเปลี่ยนคลาส
        local args = {
            [1] = className,  -- คลาสที่เลือก
            [2] = false       -- ค่าเพิ่มเติม (เช่นอาจไม่ใช้)
        }
        game:GetService("ReplicatedStorage"):WaitForChild("RemoteFunctions"):WaitForChild("ChangeClass"):InvokeServer(unpack(args))
    end)
end
