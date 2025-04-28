-- โหลด Rayfield
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- สร้างหน้าต่าง
local Window = Rayfield:CreateWindow({
    Name = "FilmXr diwaaa",
    LoadingTitle = "Class Selector",
    LoadingSubtitle = "Made by FilmXr01",
    Theme = "Default",
    DisableRayfieldPrompts = true,
})

-- สร้างแท็บ
local ClassTab = Window:CreateTab("Class", 4483362458) -- สามารถเปลี่ยน Icon ได้ตามต้องการ

-- รายชื่อ Class
local classNames = {
    "Assassin", "Atomic Samurai", "Viking", "Vampire", "Marauder",
    "Knight", "Kaiju", "Guardian", "Fighter", "Dark Mage", "Berserker", "FilmXr01Class"
}

-- สร้างปุ่มให้เลือก Class
for _, className in ipairs(classNames) do
    ClassTab:CreateButton({
        Name = className,
        Callback = function()
            local args = {
                [1] = className,
                [2] = false
            }
            game:GetService("ReplicatedStorage"):WaitForChild("RemoteFunctions"):WaitForChild("ChangeClass"):InvokeServer(unpack(args))
        end,
    })
end
