--[[

SHARK X HUB NO 1	|	https://www.youtube.com/channel/UCX--xBqSg1IdzLkaqmLTorA
SHARK X HUB NO 1	|	https://www.youtube.com/channel/UCX--xBqSg1IdzLkaqmLTorA
SHARK X HUB NO 1	|	https://www.youtube.com/channel/UCX--xBqSg1IdzLkaqmLTorA


-- Copy Position
setclipboard(tostring(game.Players.LocalPlayer.Character.HumanoidRootPart.Position))

-- Remote Spy
loadstring(game:HttpGet("https://github.com/exxtremestuffs/SimpleSpySource/raw/master/SimpleSpy.lua"))()

]]--



_G.AutoFarm_Level = true
_G.FastAttack = true









function checklevel()
    local Level = game:GetService("Players").LocalPlayer.Data.Level.Value
    if Level == 1 or Level <= 9 then
        MON = "Bandit [Lv. 5]"
        QUESTTITLE = "Bandit"
        QUESTPOS = CFrame.new(1060.0158691406, 16.424287796021, 1547.9769287109)
        MONPOS = CFrame.new(1148.8698730469, 16.432844161987, 1630.5396728516)
        QUESTNAME = "BanditQuest1"
        QUESTNUMBER = 1
        SPAWNPOINT = "Default"
        SPAWNPOINTPOS = CFrame.new(973.96197509766, 16.273551940918, 1413.2775878906)
    end
end
Method = CFrame.new(0,25,0)

spawn(function()
   while wait(3) do
       if Methodnow == 1 then
        Methodnow = 2
        Method = CFrame.new(0,25,0)
        else
        Methodnow = 1
        Method = CFrame.new(0,0,25)
       end
    end
end)

spawn(function()
   while wait() do
       if _G.WARP then
           game.Players.LocalPlayer.Character.HumanoidRootPart.Anchored = true
        else
            game.Players.LocalPlayer.Character.HumanoidRootPart.Anchored = false
        end
    end
end)

spawn(function()
    game:GetService("RunService").Heartbeat:Connect(function()
        if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Humanoid") and _G.AutoFarm_Level then
            setfflag("HumanoidParallelRemoveNoPhysics", "False")
            setfflag("HumanoidParallelRemoveNoPhysicsNoSimulate2", "False")
            game:GetService("Players").LocalPlayer.Character.Humanoid:ChangeState(11)
        end
    end)
end)


spawn(function()
    while wait() do
        if _G.AutoFarm_Level then
            pcall(function()
                checklevel()
    
                if not string.find(game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Container.QuestTitle.Title.Text,QUESTTITLE) then
                    local args = {
                        [1] = "AbandonQuest"
                    }
                    game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
                end
                if game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false then
                    
                    if game:GetService("Players").LocalPlayer.Data.SpawnPoint.Value == SPAWNPOINT then
                        local args = {
                            [1] = "SetTeam",
                            [2] = "Pirates"
                        }
                        game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
                        wait(0.5)
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = QUESTPOS
                        wait(0.8)
                            local args = {
                                [1] = "StartQuest",
                                [2] = QUESTNAME,
                                [3] = QUESTNUMBER
                            }
                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer(unpack(args))
                        wait(0.8)
                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = MONPOS
                    else
                        _G.WARP = true
                        repeat 
                            wait()
                            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = SPAWNPOINTPOS
                            game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("SetSpawnPoint")
                        until game:GetService("Players").LocalPlayer.Data.SpawnPoint.Value == SPAWNPOINT or _G.AutoFarm_Level == false
                        _G.WARP = false
                    end
                end
                for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                    for i2,v2 in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
                        if v.Name == MON and v2.Name == MON then
                            v2.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame
                            v2.HumanoidRootPart.CanCollide = false
                            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.HumanoidRootPart.CFrame * Method
                            sethiddenproperty(game.Players.LocalPlayer, "SimulationRadius", math.huge)
                        end
                    end
                end
            end)
        end
    end
end)



spawn(function()
   game:GetService("RunService").RenderStepped:Connect(function()
    pcall(function()
        if _G.FastAttack and _G.AutoFarm_Level then
            local Combat = require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework)
            local Cemara = require(game:GetService("Players").LocalPlayer.PlayerScripts.CombatFramework.CameraShaker)
            Cemara.CameraShakeInstance.CameraShakeState = {FadingIn = 3, FadingOut = 2, Sustained = 0, Inactive = 1}
            Combat.activeController.timeToNextAttack = 0
            Combat.activeController.hitboxMagnitude = 120
            Combat.activeController.increment = 3
        end
    end)
end) 
end)


spawn(function()
   game:GetService("RunService").RenderStepped:Connect(function()
    pcall(function()
        if _G.AutoFarm_Level then
            game:GetService'VirtualUser':CaptureController()
            game:GetService'VirtualUser':Button1Down(Vector2.new(1280, 672))
        end
    end)
end) 
end)
