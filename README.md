local Rayfield = loadstring(game:HttpGet("https://gitee.com/roblox-smk/rayfield/raw/master/Rayfield"))()

local Window =
    Rayfield:CreateWindow(
    {
        Name = "StarLight",
        Icon = 0,
        LoadingTitle = "加载中",
        LoadingSubtitle = "小枫制作",
        Theme = "Default",
        DisableRayfieldPrompts = false,
        DisableBuildWarnings = false,
        ConfigurationSaving = {
            Enabled = false,
            FolderName = nil,
            FileName = ""
        },
        Discord = {
            Enabled = false,
            Invite = "",
            RememberJoins = true
        },
        KeySystem = false,
        KeySettings = {
            Title = "",
            Subtitle = "",
            Note = "",
            FileName = "",
            SaveKey = true,
            GrabKeyFromSite = false,
            Key = {""}
        }
    }
)

local MainTab = Window:CreateTab("主页")

local Button =
    MainTab:CreateButton(
    {
        Name = "关闭",
        Callback = function()
            Rayfield:Destroy()
        end
    }
)

local Criminality = Window:CreateTab("犯罪")

local Button =
    Criminality:CreateButton(
    {
        Name = "秒互动|Sec interaction",
        Callback = function()
            for _, instance in ipairs(workspace:GetDescendants()) do
                if instance:IsA("ProximityPrompt") then
                    instance.HoldDuration = 0
                end
            end
        end
    }
)

local Button =
    Criminality:CreateButton(
    {
        Name = "高亮|Highlight",
        Callback = function()
            local lighting = game.Lighting
            lighting.Brightness = 2
            lighting.ClockTime = 14
            lighting.FogEnd = 100000
            lighting.GlobalShadows = false
            lighting.OutdoorAmbient = Color3.fromRGB(128, 128, 128)
        end
    }
)

local drawUserNamesT
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local StarterGui = game:GetService("StarterGui")
local billboardGuis = {}
local function onPlayerAdded(player)
    local function onCharacterAdded(character)
        local head = character:FindFirstChild("Head")
        if head then
            local billboardGui = Instance.new("BillboardGui")
            billboardGui.Parent = head
            billboardGui.AlwaysOnTop = true
            billboardGui.Size = UDim2.new(0, 200, 0, 50)
            billboardGui.StudsOffset = Vector3.new(0, 3, 0)
            local textLabel = Instance.new("TextLabel")
            textLabel.Parent = billboardGui
            textLabel.Size = UDim2.new(1, 0, 1, 0)
            textLabel.BackgroundTransparency = 1
            textLabel.TextColor3 = Color3.new(1, 1, 1)
            textLabel.Font = Enum.Font.SourceSansBold
            textLabel.TextSize = 14
            textLabel.TextStrokeTransparency = 0.3
            textLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
            textLabel.Text = player.Name
            table.insert(billboardGuis, billboardGui)
        end
    end
    player.CharacterAdded:Connect(onCharacterAdded)
end

local function Drawusernames()
    for _, player in pairs(Players:GetPlayers()) do
        if player ~= Players.LocalPlayer then
            local character = player.Character
            if character then
                local head = character:FindFirstChild("Head")
                if head then
                    local billboardGui = Instance.new("BillboardGui")
                    billboardGui.Parent = head
                    billboardGui.AlwaysOnTop = true
                    billboardGui.Size = UDim2.new(0, 200, 0, 50)
                    billboardGui.StudsOffset = Vector3.new(0, 6, 0)
                    local textLabel = Instance.new("TextLabel")
                    textLabel.Parent = billboardGui
                    textLabel.Size = UDim2.new(1, 0, 1, 0)
                    textLabel.BackgroundTransparency = 1
                    textLabel.TextColor3 = Color3.new(1, 1, 1)
                    textLabel.Font = Enum.Font.SourceSansBold
                    textLabel.TextSize = 14
                    textLabel.TextStrokeTransparency = 0.3
                    textLabel.TextStrokeColor3 = Color3.new(0, 0, 0)
                    textLabel.Text = player.Name
                    table.insert(billboardGuis, billboardGui)
                end
            end
        end
    end
end

local function RemoveDrawusernames()
    for _, gui in pairs(billboardGuis) do
        gui:Destroy()
    end
    billboardGuis = {}
end

local Toggle =
    Criminality:CreateToggle(
    {
        Name = "绘制用户名",
        CurrentValue = false,
        Flag = "",
        Callback = function(Value)
            DrawusernamesT = Value
            if Value == true then
                while DrawusernamesT == true do
                    task.wait()
                    Drawusernames()
                    wait(10)
                    RemoveDrawusernames()
                end
            else
                RemoveDrawusernames()
            end
        end
    }
)

local Lockedplayer = nil
local AimbottoggleMoble = false
local Aimbottoggle = false
local Aimbotrange = 250
local AimbotY = 0
local Teamcheak = false
local WitheList1
local WitheList2
local WitheList3
local WitheList4
local WitheList5

local function findClosestPlayerToScreenCenter()
    local players = game.Players:GetPlayers()
    local localPlayer = game.Players.LocalPlayer
    local character = localPlayer.Character
    local humanoidRootPart = character and character:FindFirstChild("HumanoidRootPart")
    if not humanoidRootPart then
        return nil
    end
    local camera = workspace.CurrentCamera
    local screenCenter = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)
    local closestPlayer = nil
    local closestDistance = math.huge
    for _, targetPlayer in ipairs(players) do
        if
            targetPlayer ~= localPlayer and targetPlayer.Name ~= WitheList1 and targetPlayer.Name ~= WitheList2 and
                targetPlayer.Name ~= WitheList3 and
                targetPlayer.Name ~= WitheList4 and
                targetPlayer.Name ~= WitheList5
         then
            local targetCharacter = targetPlayer.Character
            if targetCharacter then
                local targetRootPart = targetCharacter:FindFirstChild("HumanoidRootPart")
                if targetRootPart then
                    local targetScreenPos, inViewport = camera:WorldToScreenPoint(targetRootPart.Position)
                    if inViewport then
                        local distance = (screenCenter - Vector2.new(targetScreenPos.X, targetScreenPos.Y)).magnitude
                        if distance < closestDistance then
                            local targetHumanoid = targetCharacter:FindFirstChild("Humanoid")
                            if targetHumanoid and targetHumanoid.Health ~= 0 then
                                local targetTeam = targetPlayer.Team
                                local localTeam = localPlayer.Team
                                if not (Teamcheak == true and targetTeam == localTeam) then
                                    closestPlayer = targetPlayer
                                    closestDistance = distance
                                end
                            end
                        end
                    end
                end
            end
        end
    end
    return closestPlayer
end

local function lockViewToPlayer(player)
    if player then
        local character = player.Character
        if character then
            local head = character:FindFirstChild("Head")
            if head then
                local humanoid = character:FindFirstChild("Humanoid")
                if humanoid then
                    local camera = workspace.CurrentCamera
                    local headPosition = head.Position
                    local adjustedPosition = headPosition + Vector3.new(0, AimbotY, 0)
                    local newCameraCFrame = CFrame.new(camera.CFrame.Position, adjustedPosition)
                    camera.CFrame = newCameraCFrame
                end
            end
        end
    end
end

local function getDistanceBetweenPlayers(player1, player2)
    local character1 = player1.Character
    local character2 = player2.Character
    if character1 and character2 then
        local rootPart1 = character1:FindFirstChild("HumanoidRootPart")
        local rootPart2 = character2:FindFirstChild("HumanoidRootPart")
        if rootPart1 and rootPart2 and Aimbottoggle == true then
            local distance = (rootPart1.Position - rootPart2.Position).magnitude
            return distance
        end
    end
    return math.huge
end

local inputService = game:GetService("UserInputService")

inputService.InputBegan:Connect(
    function(input, gameProcessedEvent)
        if not gameProcessedEvent then
            if input.UserInputType == Enum.UserInputType.MouseButton2 and Aimbottoggle == true then
                Lockedplayer = findClosestPlayerToScreenCenter()
                if Lockedplayer then
                    local localPlayer = game.Players.LocalPlayer
                    local distance = getDistanceBetweenPlayers(localPlayer, Lockedplayer)
                    if distance < Aimbotrange and Aimbottoggle == true then
                        lockViewToPlayer(Lockedplayer)
                    end
                end
            end
        end
    end
)

inputService.InputEnded:Connect(
    function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton2 and Aimbottoggle == true then
            Lockedplayer = nil
            local localPlayer = game.Players.LocalPlayer
            local character = localPlayer.Character
            local humanoidRootPart = character and character:FindFirstChild("HumanoidRootPart")
        end
    end
)

game:GetService("RunService").RenderStepped:Connect(
    function()
        if Lockedplayer then
            local localPlayer = game.Players.LocalPlayer
            local distance = getDistanceBetweenPlayers(localPlayer, Lockedplayer)
            if distance < Aimbotrange and Aimbottoggle == true then
                lockViewToPlayer(Lockedplayer)
            end
        end
    end
)

local AimbotMoblebutton = nil
local aimbotCoroutine = nil
local isRightMouseDown = false

local function toggleText()
    if AimbotMoblebutton.Text == "开启" then
        AimbotMoblebutton.Text = "关闭"
        isRightMouseDown = true
        aimbotCoroutine =
            coroutine.create(
            function()
                while isRightMouseDown do
                    local lockedPlayer = findClosestPlayerToScreenCenter()
                    if lockedPlayer then
                        local localPlayer = game.Players.LocalPlayer
                        local distance = getDistanceBetweenPlayers(localPlayer, lockedPlayer)
                        if distance < Aimbotrange then
                            lockViewToPlayer(lockedPlayer)
                        end
                    end
                    task.wait()
                end
            end
        )
        coroutine.resume(aimbotCoroutine)
    else
        AimbotMoblebutton.Text = "开启"
        isRightMouseDown = false
        if aimbotCoroutine and coroutine.status(aimbotCoroutine) ~= "dead" then
            coroutine.yield(aimbotCoroutine)
        end
    end
end

local inputService = game:GetService("UserInputService")
inputService.InputBegan:Connect(
    function(input, gameProcessedEvent)
        if not gameProcessedEvent then
            if input.UserInputType == Enum.UserInputType.MouseButton2 and Aimbottoggle == true then
                if AimbotMoblebutton.Text == "开启" then
                    toggleText()
                end
            end
        end
    end
)

inputService.InputEnded:Connect(
    function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton2 and Aimbottoggle == true then
            if AimbotMoblebutton.Text == "关闭" then
                toggleText()
            end
            Lockedplayer = nil
            local localPlayer = game.Players.LocalPlayer
            local character = localPlayer.Character
            local humanoidRootPart = character and character:FindFirstChild("HumanoidRootPart")
        end
    end
)

game:GetService("RunService").RenderStepped:Connect(
    function()
        if Lockedplayer then
            local localPlayer = game.Players.LocalPlayer
            local distance = getDistanceBetweenPlayers(localPlayer, Lockedplayer)
            if distance < Aimbotrange and Aimbottoggle == true then
                lockViewToPlayer(Lockedplayer)
            end
        end
    end
)

local Criminality = ZiMiao:CreateSection("手机自瞄需要开启右键自瞄")

local Toggle =
    ZiMiao:CreateToggle(
    {
        Name = "右键自瞄",
        CurrentValue = false,
        Flag = "",
        Callback = function(Value)
            Aimbottoggle = Value
        end
    }
)

local Toggle =
    ZiMiao:CreateToggle(
    {
        Name = "手机自瞄",
        CurrentValue = false,
        Flag = "",
        Callback = function(Value)
            local Players = game:GetService("Players")
            local player = Players.LocalPlayer
            local playerGui = player:WaitForChild("PlayerGui")

            if not AimbotMoblebutton then
                local screenGui = Instance.new("ScreenGui")
                screenGui.Name = "AutoAimButtonGUI"
                screenGui.Parent = playerGui

                AimbotMoblebutton = Instance.new("TextButton")
                AimbotMoblebutton.Name = "AutoAimButton"
                AimbotMoblebutton.Size = UDim2.new(0, 100, 0, 100)
                AimbotMoblebutton.Position = UDim2.new(0, 0, 0.5, -50)
                AimbotMoblebutton.BackgroundColor3 = Color3.new(0.2, 0.2, 0.2)
                AimbotMoblebutton.TextColor3 = Color3.new(1, 1, 1)
                AimbotMoblebutton.Text = "开启"
                AimbotMoblebutton.Parent = screenGui

                AimbotMoblebutton.MouseButton1Click:Connect(toggleText)
            end

            if Value == true then
                AimbotMoblebutton.Visible = true
            else
                AimbotMoblebutton.Visible = false
                isRightMouseDown = false
                AimbotMoblebutton.Text = "开启"
                if aimbotCoroutine and coroutine.status(aimbotCoroutine) ~= "dead" then
                    coroutine.yield(aimbotCoroutine)
                end
            end
        end
    }
)

local Toggle =
    ZiMiao:CreateToggle(
    {
        Name = "队友检查",
        CurrentValue = false,
        Flag = "",
        Callback = function(Value)
            Teamcheak = Value
        end
    }
)

local Slider =
    ZiMiao:CreateSlider(
    {
        Name = "自瞄距离",
        Range = {0, 1000},
        Increment = 0.1,
        Suffix = "",
        CurrentValue = 250,
        Flag = "",
        Callback = function(Value)
            Aimbotrange = Value
        end
    }
)

local Slider =
    ZiMiao:CreateSlider(
    {
        Name = "自瞄Y坐标",
        Range = {-50, 50},
        Increment = 0.1,
        Suffix = "",
        CurrentValue = 0,
        Flag = "",
        Callback = function(Value)
            AimbotY = Value
        end
    }
)

local Input =
    ZiMiao:CreateInput(
    {
        Name = "白名单1",
        CurrentValue = "",
        PlaceholderText = "输入用户名",
        RemoveTextAfterFocusLost = false,
        Flag = "WhiteList1",
        Callback = function(Text)
            WitheList1 = Text
        end
    }
)

local Input =
    ZiMiao:CreateInput(
    {
        Name = "白名单2",
        CurrentValue = "",
        PlaceholderText = "输入用户名",
        RemoveTextAfterFocusLost = false,
        Flag = "WhiteList2",
        Callback = function(Text)
            WitheList2 = Text
        end
    }
)

local Input =
    ZiMiao:CreateInput(
    {
        Name = "白名单3",
        CurrentValue = "",
        PlaceholderText = "输入用户名",
        RemoveTextAfterFocusLost = false,
        Flag = "WhiteList3",
        Callback = function(Text)
            WitheList3 = Text
        end
    }
)

local Input =
    ZiMiao:CreateInput(
    {
        Name = "白名单4",
        CurrentValue = "",
        PlaceholderText = "输入用户名",
        RemoveTextAfterFocusLost = false,
        Flag = "WhiteList4",
        Callback = function(Text)
            WitheList4 = Text
        end
    }
)

local Input =
    ZiMiao:CreateInput(
    {
        Name = "白名单5",
        CurrentValue = "",
        PlaceholderText = "输入用户名",
        RemoveTextAfterFocusLost = false,
        Flag = "WhiteList5",
        Callback = function(Text)
            WitheList5 = Text
        end
    }
)

local Criminality = Window:CreateTab("犯罪")

local Criminality = MainTab:CreateSection("绘制")

local Toggle =
    Criminality:CreateToggle(
    {
        Name = "透视商人",
        CurrentValue = false,
        Flag = "",
        Callback = function(Value)
            local shopzFolder = workspace:FindFirstChild("Map") and workspace.Map:FindFirstChild("Shopz")

            if shopzFolder then
                for _, obj in ipairs(shopzFolder:GetChildren()) do
                    if Value then
                        local billboardGui = Instance.new("BillboardGui")
                        billboardGui.AlwaysOnTop = true
                        billboardGui.Size = UDim2.new(2, 0, 1, 0)
                        billboardGui.StudsOffset = Vector3.new(0, 6, 0)
                        billboardGui.Adornee = obj
                        billboardGui.Parent = obj

                        local textLabel = Instance.new("TextLabel")
                        textLabel.Size = UDim2.new(1, 0, 1, 0)
                        textLabel.BackgroundTransparency = 1
                        textLabel.TextColor3 = Color3.new(1, 1, 1)
                        textLabel.Font = Enum.Font.SourceSansBold
                        textLabel.TextSize = 12

                        if obj.Name == "ArmoryDealer" then
                            textLabel.Text = "武器店商人"
                        elseif obj.Name == "Dealer" then
                            textLabel.Text = "商人"
                        else
                            textLabel.Text = obj.Name
                        end

                        textLabel.Parent = billboardGui
                    else
                        local existingBillboardGui = obj:FindFirstChildOfClass("BillboardGui")
                        if existingBillboardGui then
                            existingBillboardGui:Destroy()
                        end
                    end
                end
            end
        end
    }
)

local Toggle =
    Criminality:CreateToggle(
    {
        Name = "透视保险",
        CurrentValue = false,
        Flag = "",
        Callback = function(Value)
            local bredMakurz = game.Workspace.Map.BredMakurz
            if bredMakurz then
                for _, model in ipairs(bredMakurz:GetChildren()) do
                    if model:IsA("Model") then
                        local posPart = model:FindFirstChild("PosPart")
                        if posPart then
                            local requiresLockpickBGUI = posPart:FindFirstChild("RequiresLockpickBGUI")
                            if requiresLockpickBGUI then
                                if Value == true then
                                    requiresLockpickBGUI.MaxDistance = 1000000000
                                else
                                    requiresLockpickBGUI.MaxDistance = 11
                                end
                            end
                        end
                    end
                end
            end
        end
    }
)
