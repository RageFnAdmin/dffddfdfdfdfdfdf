




if game.PlaceId == 621129760 then
    local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
    local venyx = library.new("Vex Hub: Kat", 5013109572)

    local section1 = venyx:addPage("Main", 8162117119)
    local section1 = section1:addSection("Main")



    section1:addButton("Silent Aim", function()
        local refreshrate = 0.01
        _G.toggled = true
        loadstring(game:HttpGet("https://raw.githubusercontent.com/venosu/kat/main/head.lua", true))()
        end)







        section1:addButton("Infinite Amo", function()
            local mt = getrawmetatable(game)
setreadonly(mt,false)
local nidx = mt.__newindex

mt.__newindex = newcclosure(function(a,b,c)
    if b == 'Text' and tostring(a.Name) == 'AmmoCounter' then
        return nidx(a,b,math.huge..'/3')
    end
   
    return nidx(a,b,c)
end)

local function Ammo()
    for i, v in next, getgc(true) do
        if type(v) == "table" and rawget(v, "LoadedAmmo") then
            v.LoadedAmmo = math.huge
        end
    end
end

while wait(1) do
    if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild('Revolver') then
        spawn(function()
            pcall(function()
                game.Players.LocalPlayer.Character.Revolver.ClientEvent:FireServer("SetVisible", {[1] = false})
               
                wait(0.25)
               
                game.Players.LocalPlayer.Character.Revolver.ClientEvent:FireServer("SetVisible", {[1] = true})
                game.Players.LocalPlayer.Character.Revolver.ClientEvent:FireServer("ReloadUpdate", {[1] = "Start"})
               
                wait(0.25)
               
                game.Players.LocalPlayer.Character.Revolver.ClientEvent:FireServer("ReloadUpdate", {[1] = "AmmoAdded"})
               
                Ammo()
            end)
        end)
    end
end
            end)




            section1:addButton("Esp", function()
                --[[
A distribution of https://wearedevs.net/scripts
Created August 17, 2021, Last updated August 17, 2021

Description: Draws boxes around each player.

Credits to "Real Panda" for their ESP library

Instruction: Edit the settings as desired below and execute the script.

Settings: 
Replace "nil" with "true" to enable the setting, or "false" to disable the setting. Without the quotes. 
If you do not change "nil", the defaults will take place.
]]
_G.WRDESPEnabled = nil --Enables the ESP (Defaults to true)
_G.WRDESPBoxes = nil --Draws boxes around other players (Defaults to true)
_G.WRDESPTeamColors = nil --Distinguish different teams by their team color. If the game sets one. (Defaults to true)
_G.WRDESPTracers = nil --Displays lines leading to other players (Defaults to false)
_G.WRDESPNames = nil --Displays the names of the players within the ESP box (Defaults to true)

--Dont edit below

--Only ever load the script once
if not _G.WRDESPLoaded then    
    ----[[ First- Load Kiriot ESP Library ]]----

    --Settings--
    local ESP = {
        Enabled = false,
        Boxes = true,
        BoxShift = CFrame.new(0,-1.5,0),
        BoxSize = Vector3.new(4,6,0),
        Color = Color3.fromRGB(255, 170, 0),
        FaceCamera = false,
        Names = true,
        TeamColor = true,
        Thickness = 2,
        AttachShift = 1,
        TeamMates = true,
        Players = true,
        
        Objects = setmetatable({}, {__mode="kv"}),
        Overrides = {}
    }

    --Declarations--
    local cam = workspace.CurrentCamera
    local plrs = game:GetService("Players")
    local plr = plrs.LocalPlayer
    local mouse = plr:GetMouse()

    local V3new = Vector3.new
    local WorldToViewportPoint = cam.WorldToViewportPoint

    --Functions--
    local function Draw(obj, props)
        local new = Drawing.new(obj)
        
        props = props or {}
        for i,v in pairs(props) do
            new[i] = v
        end
        return new
    end

    function ESP:GetTeam(p)
        local ov = self.Overrides.GetTeam
        if ov then
            return ov(p)
        end
        
        return p and p.Team
    end

    function ESP:IsTeamMate(p)
        local ov = self.Overrides.IsTeamMate
        if ov then
            return ov(p)
        end
        
        return self:GetTeam(p) == self:GetTeam(plr)
    end

    function ESP:GetColor(obj)
        local ov = self.Overrides.GetColor
        if ov then
            return ov(obj)
        end
        local p = self:GetPlrFromChar(obj)
        return p and self.TeamColor and p.Team and p.Team.TeamColor.Color or self.Color
    end

    function ESP:GetPlrFromChar(char)
        local ov = self.Overrides.GetPlrFromChar
        if ov then
            return ov(char)
        end
        
        return plrs:GetPlayerFromCharacter(char)
    end

    function ESP:Toggle(bool)
        self.Enabled = bool
        if not bool then
            for i,v in pairs(self.Objects) do
                if v.Type == "Box" then --fov circle etc
                    if v.Temporary then
                        v:Remove()
                    else
                        for i,v in pairs(v.Components) do
                            v.Visible = false
                        end
                    end
                end
            end
        end
    end

    function ESP:GetBox(obj)
        return self.Objects[obj]
    end

    function ESP:AddObjectListener(parent, options)
        local function NewListener(c)
            if type(options.Type) == "string" and c:IsA(options.Type) or options.Type == nil then
                if type(options.Name) == "string" and c.Name == options.Name or options.Name == nil then
                    if not options.Validator or options.Validator(c) then
                        local box = ESP:Add(c, {
                            PrimaryPart = type(options.PrimaryPart) == "string" and c:WaitForChild(options.PrimaryPart) or type(options.PrimaryPart) == "function" and options.PrimaryPart(c),
                            Color = type(options.Color) == "function" and options.Color(c) or options.Color,
                            ColorDynamic = options.ColorDynamic,
                            Name = type(options.CustomName) == "function" and options.CustomName(c) or options.CustomName,
                            IsEnabled = options.IsEnabled,
                            RenderInNil = options.RenderInNil
                        })
                        --TODO: add a better way of passing options
                        if options.OnAdded then
                            coroutine.wrap(options.OnAdded)(box)
                        end
                    end
                end
            end
        end

        if options.Recursive then
            parent.DescendantAdded:Connect(NewListener)
            for i,v in pairs(parent:GetDescendants()) do
                coroutine.wrap(NewListener)(v)
            end
        else
            parent.ChildAdded:Connect(NewListener)
            for i,v in pairs(parent:GetChildren()) do
                coroutine.wrap(NewListener)(v)
            end
        end
    end

    local boxBase = {}
    boxBase.__index = boxBase

    function boxBase:Remove()
        ESP.Objects[self.Object] = nil
        for i,v in pairs(self.Components) do
            v.Visible = false
            v:Remove()
            self.Components[i] = nil
        end
    end

    function boxBase:Update()
        if not self.PrimaryPart then
            --warn("not supposed to print", self.Object)
            return self:Remove()
        end

        local color
        if ESP.Highlighted == self.Object then
        color = ESP.HighlightColor
        else
            color = self.Color or self.ColorDynamic and self:ColorDynamic() or ESP:GetColor(self.Object) or ESP.Color
        end

        local allow = true
        if ESP.Overrides.UpdateAllow and not ESP.Overrides.UpdateAllow(self) then
            allow = false
        end
        if self.Player and not ESP.TeamMates and ESP:IsTeamMate(self.Player) then
            allow = false
        end
        if self.Player and not ESP.Players then
            allow = false
        end
        if self.IsEnabled and (type(self.IsEnabled) == "string" and not ESP[self.IsEnabled] or type(self.IsEnabled) == "function" and not self:IsEnabled()) then
            allow = false
        end
        if not workspace:IsAncestorOf(self.PrimaryPart) and not self.RenderInNil then
            allow = false
        end

        if not allow then
            for i,v in pairs(self.Components) do
                v.Visible = false
            end
            return
        end

        if ESP.Highlighted == self.Object then
            color = ESP.HighlightColor
        end

        --calculations--
        local cf = self.PrimaryPart.CFrame
        if ESP.FaceCamera then
            cf = CFrame.new(cf.p, cam.CFrame.p)
        end
        local size = self.Size
        local locs = {
            TopLeft = cf * ESP.BoxShift * CFrame.new(size.X/2,size.Y/2,0),
            TopRight = cf * ESP.BoxShift * CFrame.new(-size.X/2,size.Y/2,0),
            BottomLeft = cf * ESP.BoxShift * CFrame.new(size.X/2,-size.Y/2,0),
            BottomRight = cf * ESP.BoxShift * CFrame.new(-size.X/2,-size.Y/2,0),
            TagPos = cf * ESP.BoxShift * CFrame.new(0,size.Y/2,0),
            Torso = cf * ESP.BoxShift
        }

        if ESP.Boxes then
            local TopLeft, Vis1 = WorldToViewportPoint(cam, locs.TopLeft.p)
            local TopRight, Vis2 = WorldToViewportPoint(cam, locs.TopRight.p)
            local BottomLeft, Vis3 = WorldToViewportPoint(cam, locs.BottomLeft.p)
            local BottomRight, Vis4 = WorldToViewportPoint(cam, locs.BottomRight.p)

            if self.Components.Quad then
                if Vis1 or Vis2 or Vis3 or Vis4 then
                    self.Components.Quad.Visible = true
                    self.Components.Quad.PointA = Vector2.new(TopRight.X, TopRight.Y)
                    self.Components.Quad.PointB = Vector2.new(TopLeft.X, TopLeft.Y)
                    self.Components.Quad.PointC = Vector2.new(BottomLeft.X, BottomLeft.Y)
                    self.Components.Quad.PointD = Vector2.new(BottomRight.X, BottomRight.Y)
                    self.Components.Quad.Color = color
                else
                    self.Components.Quad.Visible = false
                end
            end
        else
            self.Components.Quad.Visible = false
        end

        if ESP.Names then
            local TagPos, Vis5 = WorldToViewportPoint(cam, locs.TagPos.p)
            
            if Vis5 then
                self.Components.Name.Visible = true
                self.Components.Name.Position = Vector2.new(TagPos.X, TagPos.Y)
                self.Components.Name.Text = self.Name
                self.Components.Name.Color = color
                
                self.Components.Distance.Visible = true
                self.Components.Distance.Position = Vector2.new(TagPos.X, TagPos.Y + 14)
                self.Components.Distance.Text = math.floor((cam.CFrame.p - cf.p).magnitude) .."m away"
                self.Components.Distance.Color = color
            else
                self.Components.Name.Visible = false
                self.Components.Distance.Visible = false
            end
        else
            self.Components.Name.Visible = false
            self.Components.Distance.Visible = false
        end
        
        if ESP.Tracers then
            local TorsoPos, Vis6 = WorldToViewportPoint(cam, locs.Torso.p)

            if Vis6 then
                self.Components.Tracer.Visible = true
                self.Components.Tracer.From = Vector2.new(TorsoPos.X, TorsoPos.Y)
                self.Components.Tracer.To = Vector2.new(cam.ViewportSize.X/2,cam.ViewportSize.Y/ESP.AttachShift)
                self.Components.Tracer.Color = color
            else
                self.Components.Tracer.Visible = false
            end
        else
            self.Components.Tracer.Visible = false
        end
    end

    function ESP:Add(obj, options)
        if not obj.Parent and not options.RenderInNil then
            return warn(obj, "has no parent")
        end

        local box = setmetatable({
            Name = options.Name or obj.Name,
            Type = "Box",
            Color = options.Color --[[or self:GetColor(obj)]],
            Size = options.Size or self.BoxSize,
            Object = obj,
            Player = options.Player or plrs:GetPlayerFromCharacter(obj),
            PrimaryPart = options.PrimaryPart or obj.ClassName == "Model" and (obj.PrimaryPart or obj:FindFirstChild("HumanoidRootPart") or obj:FindFirstChildWhichIsA("BasePart")) or obj:IsA("BasePart") and obj,
            Components = {},
            IsEnabled = options.IsEnabled,
            Temporary = options.Temporary,
            ColorDynamic = options.ColorDynamic,
            RenderInNil = options.RenderInNil
        }, boxBase)

        if self:GetBox(obj) then
            self:GetBox(obj):Remove()
        end

        box.Components["Quad"] = Draw("Quad", {
            Thickness = self.Thickness,
            Color = color,
            Transparency = 1,
            Filled = false,
            Visible = self.Enabled and self.Boxes
        })
        box.Components["Name"] = Draw("Text", {
            Text = box.Name,
            Color = box.Color,
            Center = true,
            Outline = true,
            Size = 19,
            Visible = self.Enabled and self.Names
        })
        box.Components["Distance"] = Draw("Text", {
            Color = box.Color,
            Center = true,
            Outline = true,
            Size = 19,
            Visible = self.Enabled and self.Names
        })
        
        box.Components["Tracer"] = Draw("Line", {
            Thickness = ESP.Thickness,
            Color = box.Color,
            Transparency = 1,
            Visible = self.Enabled and self.Tracers
        })
        self.Objects[obj] = box
        
        obj.AncestryChanged:Connect(function(_, parent)
            if parent == nil and ESP.AutoRemove ~= false then
                box:Remove()
            end
        end)
        obj:GetPropertyChangedSignal("Parent"):Connect(function()
            if obj.Parent == nil and ESP.AutoRemove ~= false then
                box:Remove()
            end
        end)

        local hum = obj:FindFirstChildOfClass("Humanoid")
        if hum then
            hum.Died:Connect(function()
                if ESP.AutoRemove ~= false then
                    box:Remove()
                end
            end)
        end

        return box
    end

    local function CharAdded(char)
        local p = plrs:GetPlayerFromCharacter(char)
        if not char:FindFirstChild("HumanoidRootPart") then
            local ev
            ev = char.ChildAdded:Connect(function(c)
                if c.Name == "HumanoidRootPart" then
                    ev:Disconnect()
                    ESP:Add(char, {
                        Name = p.Name,
                        Player = p,
                        PrimaryPart = c
                    })
                end
            end)
        else
            ESP:Add(char, {
                Name = p.Name,
                Player = p,
                PrimaryPart = char.HumanoidRootPart
            })
        end
    end
    local function PlayerAdded(p)
        p.CharacterAdded:Connect(CharAdded)
        if p.Character then
            coroutine.wrap(CharAdded)(p.Character)
        end
    end
    plrs.PlayerAdded:Connect(PlayerAdded)
    for i,v in pairs(plrs:GetPlayers()) do
        if v ~= plr then
            PlayerAdded(v)
        end
    end

    game:GetService("RunService").RenderStepped:Connect(function()
        cam = workspace.CurrentCamera
        for i,v in (ESP.Enabled and pairs or ipairs)(ESP.Objects) do
            if v.Update then
                local s,e = pcall(v.Update, v)
                if not s then warn("[EU]", e, v.Object:GetFullName()) end
            end
        end
    end)

    ----[[ Now Begins WRD's modification for implementation ]]----

    --Sets defaults where required
    if _G.WRDESPEnabled == nil then _G.WRDESPEnabled = true end
    if _G.WRDESPBoxes == nil then _G.WRDESPBoxes = true end
    if _G.WRDESPTeamColors == nil then _G.WRDESPTeamColors = true end
    if _G.WRDESPTracers == nil then _G.WRDESPTracers = false end
    if _G.WRDESPNames == nil then _G.WRDESPNames = true end
	
	--Hacky way to keep up with setting changes
    while wait(.1) do
        ESP:Toggle(_G.WRDESPEnabled or false)
        ESP.Boxes = _G.WRDESPBoxes or false
        ESP.TeamColors = _G.WRDESPTeamColors or false
        ESP.Tracers = _G.WRDESPTracers or false
        ESP.Names = _G.WRDESPNames or false
    end

    _G.WRDESPLoaded = true
end
            end)





















            local page2 = venyx:addPage("Local Player", 8097641254)
            local Farm = page2:addSection("Local Player")
    

    
            local Farm = page2:addSection("Infinite Jump")
        local RANKUP
        Farm:addToggle("Inf Jump", nil, function(bool)
            game:GetService("UserInputService").JumpRequest:connect(function()
                game:GetService"Players".LocalPlayer.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")      
            end)
            RANKUP = bool
        end)
    
    
    
    
    
        local Farm = page2:addSection("Anti-Afk")
        local RANKUP
        Farm:addToggle("Anti-Afk", nil, function(bool)
            wait(0.5)local ba=Instance.new("ScreenGui")
            local ca=Instance.new("TextLabel")local da=Instance.new("Frame")
            local _b=Instance.new("TextLabel")local ab=Instance.new("TextLabel")ba.Parent=game.CoreGui
            ba.ZIndexBehavior=Enum.ZIndexBehavior.Sibling;ca.Parent=ba;ca.Active=true
            ca.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ca.Draggable=true
            ca.Position=UDim2.new(0.698610067,0,0.098096624,0)ca.Size=UDim2.new(0,304,0,52)
            ca.Font=Enum.Font.SourceSansSemibold;ca.Text="Anti Afk Kick Script"ca.TextColor3=Color3.new(0,1,1)
            ca.TextSize=22;da.Parent=ca
            da.BackgroundColor3=Color3.new(0.196078,0.196078,0.196078)da.Position=UDim2.new(0,0,1.0192306,0)
            da.Size=UDim2.new(0,304,0,107)_b.Parent=da
            _b.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)_b.Position=UDim2.new(0,0,0.800455689,0)
            _b.Size=UDim2.new(0,304,0,21)_b.Font=Enum.Font.Arial;_b.Text="Made by: RealLeon#0005"
            _b.TextColor3=Color3.new(1,1,1)_b.TextSize=20;ab.Parent=da
            ab.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ab.Position=UDim2.new(0,0,0.158377379,0)
            ab.Size=UDim2.new(0,304,0,44)ab.Font=Enum.Font.ArialBold;ab.Text="Status: Script Started"
            ab.TextColor3=Color3.new(1,1,1)ab.TextSize=20;local bb=game:service'VirtualUser'
            game:service'Players'.LocalPlayer.Idled:connect(function()
            bb:CaptureController()bb:ClickButton2(Vector2.new())
            ab.Text="You went idle and ROBLOX tried to kick you but we reflected it!"wait(2)ab.Text="Script Re-Enabled"end)
            RANKUP = bool
        end)
    
    
    
        local Farm = page2:addSection("Fly with (E)")
        local RANKUP
        Farm:addToggle("Fly (E)", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/7rXZ9VNc", true))()
            RANKUP = bool
        end)
    
    



        local Title = page2:addSection("Reset Character")
    
    
        Title:addButton("Reset", function()
            game.Players.LocalPlayer.Character.Head:Remove()
        end)
    
    



    
    
        local Farm = page2:addSection("Noclip with (R)")
        local RANKUP
        Farm:addToggle("Noclip (R)", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/2pwTjwS4", true))()		
            RANKUP = bool
        end)
    



    
    
    
        local Farm = page2:addSection("Alt Delete")
        local RANKUP
        Farm:addToggle("Alt Delet", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/DThr62Cn", true))()		
            RANKUP = bool
        end)
    
    
    
    
        local Farm = page2:addSection("Ctrl Teleport")
        local RANKUP
        Farm:addToggle("Ctrl Teleport", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/gp9y2Wht", true))()	
            RANKUP = bool
        end)
    
    
    
        local Farm = page2:addSection("God Mode")
        local RANKUP
        Farm:addToggle("God Mode", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/MSZAznxp", true))()
            RANKUP = bool
        end)
















            local page5 = venyx:addPage("Credits", 8086705745)
            local Discord = page5:addSection("Discord")
            
            
            Discord:addButton("Copy Discord Link", function()
                setclipboard("https://discord.gg/JdEK5qN7Yb")
            end)
            local Discord = page5:addSection("Credits")
            local d = page5:addSection("Made by: Leon!#0831")
            local d = page5:addSection("Scripter: Leon!#0831")
        
        
        
        
        
        
        
            -- themes
            local themes = {
                Background = Color3.fromRGB(24, 24, 24),
                Glow = Color3.fromRGB(0, 0, 0),
                Accent = Color3.fromRGB(10, 10, 10),
                LightContrast = Color3.fromRGB(20, 20, 20),
                DarkContrast = Color3.fromRGB(14, 14, 14),
                TextColor = Color3.fromRGB(255, 255, 255)
                }
                
                    
                -- Theme page
                local settings = venyx:addPage("Settings", 8087082683)
                local colors = settings:addSection("Colors")
                local setting = settings:addSection("Settings")
                
                setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
                print("Activated Keybind")
                venyx:toggle()
                end, function()
                print("Changed Keybind")
                end)
                
                for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
                colors:addColorPicker(theme, color, function(color3)
                venyx:setTheme(theme, color3)
                end)
                end
            
            
            
            
            
            
            
            
            
            
            
            -- load
                venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
                end







































        if game.PlaceId == 286090429 then
    
    
    
    
    
    
    local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
    local venyx = library.new("Vex Hub: Arsenal", 5013109572)
    
    
    
    
    
    
    
    
    
    
    
    local page2 = venyx:addPage("Main", 5012544693)
    local Farm = page2:addSection("Aimbot")
    
    
    
    
    
    local RANKUP
        Farm:addToggle("Anti-Aim", nil, function(bool)
            loadstring(game:HttpGet("https://raw.githubusercontent.com/The3Bakers4565/Spicy-Bagel/main/Other_Scripts/Arsenal/Anti_Aim.lua"))()
            RANKUP = bool
        end)
    
    
        
    
    
    
    
    
    local RANKUP
        Farm:addToggle("Shoot To Kill All The Bitches", nil, function(bool)
            loadstring(game:HttpGet("https://raw.githubusercontent.com/GameEditor-RBLX23/momentofcheese/main/cheeeeeseweee", true))()
            RANKUP = bool
        end)
    
    
    
    
    
    
    
    
    
    local RANKUP
        Farm:addToggle("Silent Aim", nil, function(bool)
            loadstring(game:HttpGet("https://raw.githubusercontent.com/GameEditor-RBLX23/GameEditor/main/Script", true))()
            RANKUP = bool
        end)
    
    
    
    
    
    
    
    
    
    local RANKUP
        Farm:addToggle("Wall Bang", nil, function(bool)
            _G.Enable = true
    local MT = getrawmetatable(game)
    local OldIndex = MT.__index
    setreadonly(MT, false)
    MT.__index = newcclosure(function(A, B)
        if B == "Clips" then
            if _G.Enable then
                return workspace.Map
            end
        end
        return OldIndex(A, B)
    end)
            RANKUP = bool
        end)
    
    
    
    
    
    
    
    
    
    
    
    
    
    local Aimbot
        Farm:addToggle("Aimbot + Fov", nil, function(bool)
            local Camera = workspace.CurrentCamera
    local Players = game:GetService("Players")
    local RunService = game:GetService("RunService")
    local UserInputService = game:GetService("UserInputService")
    local TweenService = game:GetService("TweenService")
    local LocalPlayer = Players.LocalPlayer
    local Holding = false
    local Locked = false
    local Victim
     
    _G.AimbotEnabled = true
    _G.TeamCheck = true -- If set to true then the script would only lock your aim at enemy team members.
    _G.AimPart = "Head" -- Where the aimbot script would lock at.
    _G.Sensitivity = 0 -- How many seconds it takes for the aimbot script to officially lock onto the target's aimpart.
     
    _G.CircleSides = 188 -- How many sides the FOV circle would have.
    _G.CircleColor = Color3.fromRGB(255, 255, 255) -- (RGB) Color that the FOV circle would appear as.
    _G.CircleTransparency = 0.7 -- Transparency of the circle.
    _G.CircleRadius = 250 -- The radius of the circle / FOV.
    _G.CircleFilled = false -- Determines whether or not the circle is filled.
    _G.CircleVisible = true -- Determines whether or not the circle is visible.
    _G.CircleThickness = 0 -- The thickness of the circle.
     
    local FOVCircle = Drawing.new("Circle")
    FOVCircle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    FOVCircle.Radius = _G.CircleRadius
    FOVCircle.Filled = _G.CircleFilled
    FOVCircle.Color = _G.CircleColor
    FOVCircle.Visible = _G.CircleVisible
    FOVCircle.Radius = _G.CircleRadius
    FOVCircle.Transparency = _G.CircleTransparency
    FOVCircle.NumSides = _G.CircleSides
    FOVCircle.Thickness = _G.CircleThickness
     
    local function GetClosestPlayer()
        local MaximumDistance = _G.CircleRadius
        local Target
     
        for _, v in pairs(game.Players:GetPlayers()) do
            if v.Name ~= LocalPlayer.Name then
                if _G.TeamCheck == true then 
                    if v.Team ~= LocalPlayer.Team then
                        if workspace:FindFirstChild(v.Name) ~= nil then
                            if workspace[v.Name]:FindFirstChild("HumanoidRootPart") ~= nil then
                                if workspace[v.Name]:FindFirstChild("Humanoid") ~= nil and workspace[v.Name]:FindFirstChild("Humanoid").Health ~= 0 then
                                    local ScreenPoint = Camera:WorldToScreenPoint(workspace[v.Name]:WaitForChild("HumanoidRootPart", math.huge).Position)
                                    local VectorDistance = (Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y) - Vector2.new(ScreenPoint.X, ScreenPoint.Y)).Magnitude
     
                                    if VectorDistance < MaximumDistance then
                                        Target = v
                                    end
                                end
                            end
                        end
                    end
                else
                    if workspace:FindFirstChild(v.Name) ~= nil then
                        if workspace[v.Name]:FindFirstChild("HumanoidRootPart") ~= nil then
                            if workspace[v.Name]:FindFirstChild("Humanoid") ~= nil and workspace[v.Name]:FindFirstChild("Humanoid").Health ~= 0 then
                                local ScreenPoint = Camera:WorldToScreenPoint(workspace[v.Name]:WaitForChild("HumanoidRootPart", math.huge).Position)
                                local VectorDistance = (Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y) - Vector2.new(ScreenPoint.X, ScreenPoint.Y)).Magnitude
     
                                if VectorDistance < MaximumDistance then
                                    Target = v
                                end
                            end
                        end
                    end
                end
            end
        end
     
        return Target
    end
     
    UserInputService.InputBegan:Connect(function(Input)
        if Input.UserInputType == Enum.UserInputType.MouseButton2 then
            Holding = true
        end
    end)
     
    UserInputService.InputEnded:Connect(function(Input)
        if Input.UserInputType == Enum.UserInputType.MouseButton2 then
            Holding = false
            Locked = false
        end
    end)
     
    RunService.RenderStepped:Connect(function()
        FOVCircle.Position = Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y)
        FOVCircle.Radius = _G.CircleRadius
        FOVCircle.Filled = _G.CircleFilled
        FOVCircle.Color = _G.CircleColor
        FOVCircle.Visible = _G.CircleVisible
        FOVCircle.Radius = _G.CircleRadius
        FOVCircle.Transparency = _G.CircleTransparency
        FOVCircle.NumSides = _G.CircleSides
        FOVCircle.Thickness = _G.CircleThickness
        if Holding == true and _G.AimbotEnabled == true then
            TweenService:Create(Camera, TweenInfo.new(_G.Sensitivity, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {CFrame = CFrame.new(Camera.CFrame.Position, GetClosestPlayer().Character[_G.AimPart].Position)}):Play()
            Locked = true
        end
    end)
            Aimbot = bool
        end)
    
    
    
    
    
    
    
        
    
    
    
    
    
    
    
    
    
    
    
    
        
    local RANKUP
        Farm:addToggle("Esp", nil, function(bool)
            local Holder = Instance.new("Folder", game.CoreGui)
    Holder.Name = "ESP"
     
    local Box = Instance.new("BoxHandleAdornment")
    Box.Name = "nilBox"
    Box.Size = Vector3.new(4, 7, 4)
    Box.Color3 = Color3.new(100 / 255, 100 / 255, 100 / 255)
    Box.Transparency = 0.7
    Box.ZIndex = 0
    Box.AlwaysOnTop = true
    Box.Visible = true
     
    local NameTag = Instance.new("BillboardGui")
    NameTag.Name = "nilNameTag"
    NameTag.Enabled = false
    NameTag.Size = UDim2.new(0, 200, 0, 50)
    NameTag.AlwaysOnTop = true
    NameTag.StudsOffset = Vector3.new(0, 1.8, 0)
    local Tag = Instance.new("TextLabel", NameTag)
    Tag.Name = "Tag"
    Tag.BackgroundTransparency = 1
    Tag.Position = UDim2.new(0, -50, 0, 0)
    Tag.Size = UDim2.new(0, 300, 0, 20)
    Tag.TextSize = 20
    Tag.TextColor3 = Color3.new(100 / 255, 100 / 255, 100 / 255)
    Tag.TextStrokeColor3 = Color3.new(0 / 255, 0 / 255, 0 / 255)
    Tag.TextStrokeTransparency = 0.4
    Tag.Text = "nil"
    Tag.Font = Enum.Font.SourceSansBold
    Tag.TextScaled = false
     
    local LoadCharacter = function(v)
        repeat wait() until v.Character ~= nil
        v.Character:WaitForChild("Humanoid")
        local vHolder = Holder:FindFirstChild(v.Name)
        vHolder:ClearAllChildren()
        local b = Box:Clone()
        b.Name = v.Name .. "Box"
        b.Adornee = v.Character
        b.Parent = vHolder
        local t = NameTag:Clone()
        t.Name = v.Name .. "NameTag"
        t.Enabled = true
        t.Parent = vHolder
        t.Adornee = v.Character:WaitForChild("Head", 5)
        if not t.Adornee then
            return UnloadCharacter(v)
        end
        t.Tag.Text = v.Name
        b.Color3 = Color3.new(v.TeamColor.r, v.TeamColor.g, v.TeamColor.b)
        t.Tag.TextColor3 = Color3.new(v.TeamColor.r, v.TeamColor.g, v.TeamColor.b)
        local Update
        local UpdateNameTag = function()
            if not pcall(function()
                v.Character.Humanoid.DisplayDistanceType = Enum.HumanoidDisplayDistanceType.None
                local maxh = math.floor(v.Character.Humanoid.MaxHealth)
                local h = math.floor(v.Character.Humanoid.Health)
                t.Tag.Text = v.Name .. "\n" .. ((maxh ~= 0 and tostring(math.floor((h / maxh) * 100))) or "0") .. "%  " .. tostring(h) .. "/" .. tostring(maxh)
            end) then
                Update:Disconnect()
            end
        end
        UpdateNameTag()
        Update = v.Character.Humanoid.Changed:Connect(UpdateNameTag)
    end
     
    local UnloadCharacter = function(v)
        local vHolder = Holder:FindFirstChild(v.Name)
        if vHolder and (vHolder:FindFirstChild(v.Name .. "Box") ~= nil or vHolder:FindFirstChild(v.Name .. "NameTag") ~= nil) then
            vHolder:ClearAllChildren()
        end
    end
     
    local LoadPlayer = function(v)
        local vHolder = Instance.new("Folder", Holder)
        vHolder.Name = v.Name
        v.CharacterAdded:Connect(function()
            pcall(LoadCharacter, v)
        end)
        v.CharacterRemoving:Connect(function()
            pcall(UnloadCharacter, v)
        end)
        v.Changed:Connect(function(prop)
            if prop == "TeamColor" then
                UnloadCharacter(v)
                wait()
                LoadCharacter(v)
            end
        end)
        LoadCharacter(v)
    end
     
    local UnloadPlayer = function(v)
        UnloadCharacter(v)
        local vHolder = Holder:FindFirstChild(v.Name)
        if vHolder then
            vHolder:Destroy()
        end
    end
     
    for i,v in pairs(game:GetService("Players"):GetPlayers()) do
        spawn(function() pcall(LoadPlayer, v) end)
    end
     
    game:GetService("Players").PlayerAdded:Connect(function(v)
        pcall(LoadPlayer, v)
    end)
     
    game:GetService("Players").PlayerRemoving:Connect(function(v)
        pcall(UnloadPlayer, v)
    end)
     
    game:GetService("Players").LocalPlayer.NameDisplayDistance = 0
            RANKUP = bool
        end)
    
    
    
    
    
    
    
    
    
    
    getgenv().speed = 500
    local autofarm
        Farm:addToggle("Aimbot", nil, function(bool)
            local Camera = workspace.CurrentCamera
    local Players = game:GetService("Players")
    local RunService = game:GetService("RunService")
    local UserInputService = game:GetService("UserInputService")
    local TweenService = game:GetService("TweenService")
    local LocalPlayer = Players.LocalPlayer
    local Holding = false
    local Locked = false
    local Victim
     
    _G.AimbotEnabled = true
    _G.TeamCheck = true -- If set to true then the script would only lock your aim at enemy team members.
    _G.AimPart = "Head" -- Where the aimbot script would lock at.
    _G.Sensitivity = 0 -- How many seconds it takes for the aimbot script to officially lock onto the target's aimpart.
     
    _G.CircleSides = 188 -- How many sides the FOV circle would have.
    _G.CircleColor = Color3.fromRGB(255, 255, 255) -- (RGB) Color that the FOV circle would appear as.
    _G.CircleTransparency = 0.7 -- Transparency of the circle.
    _G.CircleRadius = 400 -- The radius of the circle / FOV.
    _G.CircleFilled = false -- Determines whether or not the circle is filled.
    _G.CircleVisible = false -- Determines whether or not the circle is visible.
    _G.CircleThickness = 0 -- The thickness of the circle.
     
    local FOVCircle = Drawing.new("Circle")
    FOVCircle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    FOVCircle.Radius = _G.CircleRadius
    FOVCircle.Filled = _G.CircleFilled
    FOVCircle.Color = _G.CircleColor
    FOVCircle.Visible = _G.CircleVisible
    FOVCircle.Radius = _G.CircleRadius
    FOVCircle.Transparency = _G.CircleTransparency
    FOVCircle.NumSides = _G.CircleSides
    FOVCircle.Thickness = _G.CircleThickness
     
    local function GetClosestPlayer()
        local MaximumDistance = _G.CircleRadius
        local Target
     
        for _, v in pairs(game.Players:GetPlayers()) do
            if v.Name ~= LocalPlayer.Name then
                if _G.TeamCheck == true then 
                    if v.Team ~= LocalPlayer.Team then
                        if workspace:FindFirstChild(v.Name) ~= nil then
                            if workspace[v.Name]:FindFirstChild("HumanoidRootPart") ~= nil then
                                if workspace[v.Name]:FindFirstChild("Humanoid") ~= nil and workspace[v.Name]:FindFirstChild("Humanoid").Health ~= 0 then
                                    local ScreenPoint = Camera:WorldToScreenPoint(workspace[v.Name]:WaitForChild("HumanoidRootPart", math.huge).Position)
                                    local VectorDistance = (Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y) - Vector2.new(ScreenPoint.X, ScreenPoint.Y)).Magnitude
     
                                    if VectorDistance < MaximumDistance then
                                        Target = v
                                    end
                                end
                            end
                        end
                    end
                else
                    if workspace:FindFirstChild(v.Name) ~= nil then
                        if workspace[v.Name]:FindFirstChild("HumanoidRootPart") ~= nil then
                            if workspace[v.Name]:FindFirstChild("Humanoid") ~= nil and workspace[v.Name]:FindFirstChild("Humanoid").Health ~= 0 then
                                local ScreenPoint = Camera:WorldToScreenPoint(workspace[v.Name]:WaitForChild("HumanoidRootPart", math.huge).Position)
                                local VectorDistance = (Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y) - Vector2.new(ScreenPoint.X, ScreenPoint.Y)).Magnitude
     
                                if VectorDistance < MaximumDistance then
                                    Target = v
                                end
                            end
                        end
                    end
                end
            end
        end
     
        return Target
    end
     
    UserInputService.InputBegan:Connect(function(Input)
        if Input.UserInputType == Enum.UserInputType.MouseButton2 then
            Holding = true
        end
    end)
     
    UserInputService.InputEnded:Connect(function(Input)
        if Input.UserInputType == Enum.UserInputType.MouseButton2 then
            Holding = false
            Locked = false
        end
    end)
     
    RunService.RenderStepped:Connect(function()
        FOVCircle.Position = Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y)
        FOVCircle.Radius = _G.CircleRadius
        FOVCircle.Filled = _G.CircleFilled
        FOVCircle.Color = _G.CircleColor
        FOVCircle.Visible = _G.CircleVisible
        FOVCircle.Radius = _G.CircleRadius
        FOVCircle.Transparency = _G.CircleTransparency
        FOVCircle.NumSides = _G.CircleSides
        FOVCircle.Thickness = _G.CircleThickness
        if Holding == true and _G.AimbotEnabled == true then
            TweenService:Create(Camera, TweenInfo.new(_G.Sensitivity, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {CFrame = CFrame.new(Camera.CFrame.Position, GetClosestPlayer().Character[_G.AimPart].Position)}):Play()
            Locked = true
        end
    end)
            autofarm = bool
        end)
    
    
    
    
        
        local Farm = page2:addSection("Visual")
        local RANKUP
        
        
        
        
        

    
    
    
    
    
    
    
    
    
    
    
    
        local Fov
        Farm:addToggle("Fov", nil, function(bool)
            _G.CircleSides = 64 -- How many sides the FOV circle would have.
    _G.CircleColor = Color3.fromRGB(255, 255, 255) -- (RGB) Color that the FOV circle would appear as.
    _G.CircleTransparency = 0.7 -- Transparency of the circle.
    _G.CircleRadius = 80 -- The radius of the circle / FOV.
    _G.CircleFilled = false -- Determines whether or not the circle is filled.
    _G.CircleVisible = true -- Determines whether or not the circle is visible.
    _G.CircleThickness = 0 -- The thickness of the circle.
     
    local FOVCircle = Drawing.new("Circle")
    FOVCircle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
    FOVCircle.Radius = _G.CircleRadius
    FOVCircle.Filled = _G.CircleFilled
    FOVCircle.Color = _G.CircleColor
    FOVCircle.Visible = _G.CircleVisible
    FOVCircle.Radius = _G.CircleRadius
    FOVCircle.Transparency = _G.CircleTransparency
    FOVCircle.NumSides = _G.CircleSides
    FOVCircle.Thickness = _G.CircleThickness
     
    local function GetClosestPlayer()
        local MaximumDistance = _G.CircleRadius
        local Target
     
        for _, v in pairs(game.Players:GetPlayers()) do
            if v.Name ~= LocalPlayer.Name then
                if _G.TeamCheck == true then 
                    if v.Team ~= LocalPlayer.Team then
                        if workspace:FindFirstChild(v.Name) ~= nil then
                            if workspace[v.Name]:FindFirstChild("HumanoidRootPart") ~= nil then
                                if workspace[v.Name]:FindFirstChild("Humanoid") ~= nil and workspace[v.Name]:FindFirstChild("Humanoid").Health ~= 0 then
                                    local ScreenPoint = Camera:WorldToScreenPoint(workspace[v.Name]:WaitForChild("HumanoidRootPart", math.huge).Position)
                                    local VectorDistance = (Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y) - Vector2.new(ScreenPoint.X, ScreenPoint.Y)).Magnitude
     
                                    if VectorDistance < MaximumDistance then
                                        Target = v
                                    end
                                end
                            end
                        end
                    end
                else
                    if workspace:FindFirstChild(v.Name) ~= nil then
                        if workspace[v.Name]:FindFirstChild("HumanoidRootPart") ~= nil then
                            if workspace[v.Name]:FindFirstChild("Humanoid") ~= nil and workspace[v.Name]:FindFirstChild("Humanoid").Health ~= 0 then
                                local ScreenPoint = Camera:WorldToScreenPoint(workspace[v.Name]:WaitForChild("HumanoidRootPart", math.huge).Position)
                                local VectorDistance = (Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y) - Vector2.new(ScreenPoint.X, ScreenPoint.Y)).Magnitude
     
                                if VectorDistance < MaximumDistance then
                                    Target = v
                                end
                            end
                        end
                    end
                end
            end
        end
     
        return Target
    end
     
    UserInputService.InputBegan:Connect(function(Input)
        if Input.UserInputType == Enum.UserInputType.MouseButton2 then
            Holding = true
        end
    end)
     
    UserInputService.InputEnded:Connect(function(Input)
        if Input.UserInputType == Enum.UserInputType.MouseButton2 then
            Holding = false
            Locked = false
        end
    end)
     
    RunService.RenderStepped:Connect(function()
        FOVCircle.Position = Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y)
        FOVCircle.Radius = _G.CircleRadius
        FOVCircle.Filled = _G.CircleFilled
        FOVCircle.Color = _G.CircleColor
        FOVCircle.Visible = _G.CircleVisible
        FOVCircle.Radius = _G.CircleRadius
        FOVCircle.Transparency = _G.CircleTransparency
        FOVCircle.NumSides = _G.CircleSides
        FOVCircle.Thickness = _G.CircleThickness
        if Holding == true and _G.AimbotEnabled == true then
            TweenService:Create(Camera, TweenInfo.new(_G.Sensitivity, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {CFrame = CFrame.new(Camera.CFrame.Position, GetClosestPlayer().Character[_G.AimPart].Position)}):Play()
            Locked = true
        end
    end)
            Fov = bool
        end)
    
        
        local RANKUP
        Farm:addToggle("One hit all", nil, function(bool)
             
    local met = getrawmetatable(game)
    setreadonly(met,false)
    local old = met.__namecall
    met.__namecall = function(t,...)
    local args = {...}
    if(getnamecallmethod()=="FireServer") then
    if(tostring(t) == "HitPart") then
      for i = 0,25 do old(t,...)end
    end
     
    end
    return old(t,...)
    end
            RANKUP = bool
        end)
        
        
        local d = page2:addSection("click first on fov + aimbot and then on fov")
    
    
    
            local Farm = page2:addSection("Kill Aura")
            local RANKUP
        Farm:addToggle("Kill Aura", nil, function(bool)
            _G.Range = 25
    
            loadstring(game:HttpGet("https://raw.githubusercontent.com/JRL-lav/Scripts/main/Arsenal"))() 
            RANKUP = bool
        end)
    
    
    
    
        
    
    
        local page3 = venyx:addPage("Local Player", 5012544693)
        local Farm = page3:addSection("Local Player")
    
        local InfJump
        Farm:addToggle("Infinite Jump", nil, function(bool)
            game:GetService("UserInputService").JumpRequest:connect(function()
                game:GetService"Players".LocalPlayer.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")      
            end)
            InfJump = bool
        end)
    
        local RANKUP
        Farm:addToggle("Fly (E)", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/7rXZ9VNc", true))()
            RANKUP = bool
        end)
    
    
        local RANKUP
        Farm:addToggle("Noclip (R)", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/2pwTjwS4", true))()
            RANKUP = bool
        end)
    
    
        local RANKUP
        Farm:addToggle("God Mode", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/MSZAznxp", true))()
            RANKUP = bool
        end)
    
    
        local RANKUP
        Farm:addToggle("Anti-Afk", nil, function(bool)
            wait(0.5)local ba=Instance.new("ScreenGui")
    local ca=Instance.new("TextLabel")local da=Instance.new("Frame")
    local _b=Instance.new("TextLabel")local ab=Instance.new("TextLabel")ba.Parent=game.CoreGui
    ba.ZIndexBehavior=Enum.ZIndexBehavior.Sibling;ca.Parent=ba;ca.Active=true
    ca.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ca.Draggable=true
    ca.Position=UDim2.new(0.698610067,0,0.098096624,0)ca.Size=UDim2.new(0,304,0,52)
    ca.Font=Enum.Font.SourceSansSemibold;ca.Text="Anti Afk Kick Script"ca.TextColor3=Color3.new(0,1,1)
    ca.TextSize=22;da.Parent=ca
    da.BackgroundColor3=Color3.new(0.196078,0.196078,0.196078)da.Position=UDim2.new(0,0,1.0192306,0)
    da.Size=UDim2.new(0,304,0,107)_b.Parent=da
    _b.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)_b.Position=UDim2.new(0,0,0.800455689,0)
    _b.Size=UDim2.new(0,304,0,21)_b.Font=Enum.Font.Arial;_b.Text="Made by: RealLeon#0005"
    _b.TextColor3=Color3.new(1,1,1)_b.TextSize=20;ab.Parent=da
    ab.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ab.Position=UDim2.new(0,0,0.158377379,0)
    ab.Size=UDim2.new(0,304,0,44)ab.Font=Enum.Font.ArialBold;ab.Text="Status: Script Started"
    ab.TextColor3=Color3.new(1,1,1)ab.TextSize=20;local bb=game:service'VirtualUser'
    game:service'Players'.LocalPlayer.Idled:connect(function()
    bb:CaptureController()bb:ClickButton2(Vector2.new())
    ab.Text="You went idle and ROBLOX tried to kick you but we reflected it!"wait(2)ab.Text="Script Re-Enabled"end)
            RANKUP = bool
        end)
    
    
    
        
    
    
    
    
        
    
        
    
    
    
    
    
    
    
    
    
        
    
        local page4 = venyx:addPage("Aim Help", 5012544693)
        local Farm = page4:addSection("Aim Help")
        local Farm = page4:addSection("Click on Aimbot and turn it on")
        local Farm = page4:addSection("Than click on play")
        local Farm = page4:addSection("Press in Game Right Click")
        local Farm = page4:addSection("Is it Works?")
        local Farm = page4:addSection("If it lags has join them my discord server-")
        local Farm = page4:addSection("and write to me then tell them what the problem is")
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    local page5 = venyx:addPage("Credits", 5012544693)
    local Discord = page5:addSection("Discord")
    
    
    Discord:addButton("Copy Discord Link", function()
        setclipboard("https://discord.gg/JdEK5qN7Yb")
    end)
    local Discord = page5:addSection("Credits")
    local d = page5:addSection("Made by: Leon!#0831")
    local d = page5:addSection("Scripter: Leon!#0831")
    
    
    
    -- themes
    local themes = {
    Background = Color3.fromRGB(24, 24, 24),
    Glow = Color3.fromRGB(0, 0, 0),
    Accent = Color3.fromRGB(10, 10, 10),
    LightContrast = Color3.fromRGB(20, 20, 20),
    DarkContrast = Color3.fromRGB(14, 14, 14),
    TextColor = Color3.fromRGB(255, 255, 255)
    }
    
        
    -- Theme page
    local settings = venyx:addPage("Settings", 5012544693)
    local colors = settings:addSection("Colors")
    local setting = settings:addSection("Settings")
    
    setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
    print("Activated Keybind")
    venyx:toggle()
    end, function()
    print("Changed Keybind")
    end)
    
    for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
    colors:addColorPicker(theme, color, function(color3)
    venyx:setTheme(theme, color3)
    end)
    end
    
    -- load
    venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
    end













































































    



    if game.PlaceId == 2788229376 then
        local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
        local venyx = library.new("Vex Hub: Da Hood", 5013109572)

        






        local page1 = venyx:addPage("Local Player", 8097641254)
        local Farm = page1:addSection("ESP")





        local RANKUP
        Farm:addToggle("Esp", nil, function(bool)
            local MainParent = game.workspace.Terrain or nil
local LineFrame = Instance.new("ScreenGui",MainParent)
LineFrame.Name = "Lines"
LineFrame.Enabled = false
ToggleKey = Enum.KeyCode.F1
local Spotted = {};
local CharacterMotors = {}
CharacterMotors.GetMotors = function(char)
    local Motors = {}
        for i,v in pairs(char:GetDescendants()) do
            if v:IsA("Motor6D") then
                if v.Part0 and v.Part1 then--and v.Part0.Name ~= "HumanoidRootPart" and v.Part1.Name ~= "HumanoidRootPart" then
                    table.insert(Motors,{v.Part0,v.Part1})
                end
            end
        end
    table.insert(Motors,{char:FindFirstChild("Head") or char.PrimaryPart,"Camera"})
    CharacterMotors[char] = Motors
    return CharacterMotors[char]
end
        
lerp = function(a, b, t)
    return a + (b - a) * t
end
 
local ESP = {Characters = {}}
ESP.Visible = true
ESP.Init = function(char,plr)
    --if (game:GetService("Players").LocalPlayer.Team == nil or plr.Team ~= game:GetService("Players").LocalPlayer.Team) then
        char.DescendantAdded:Connect(function()
            CharacterMotors.GetMotors(char)
        end)
        
        local BillboardUi = Instance.new("BillboardGui")
        BillboardUi.AlwaysOnTop = true
        BillboardUi.Size = UDim2.new(3,60,1,30)
        BillboardUi.StudsOffsetWorldSpace = Vector3.new(0,-4.5,0)
        BillboardUi.Adornee = char:WaitForChild("HumanoidRootPart")
        
        local PlayerName = Instance.new("TextLabel",BillboardUi)
        PlayerName.BackgroundTransparency = 1
        PlayerName.TextColor3 = (game:GetService("Players"):GetPlayerFromCharacter(char)).TeamColor.Color
        PlayerName.Size = UDim2.new(1,0,.3,0)
        PlayerName.ZIndex = 2
        PlayerName.Text = char.Name
        PlayerName.TextScaled = true
        PlayerName.TextStrokeTransparency = .25
        PlayerName.Font = Enum.Font.GothamBold
        PlayerName.TextStrokeColor3 = Color3.fromRGB(33, 33, 33)
        
        local Distance = PlayerName:Clone()
        Distance.Parent = BillboardUi
        Distance.Font = Enum.Font.Gotham
        Distance.TextColor3 = Color3.new(1,1,1)
        Distance.Position = UDim2.new(0,0,.3,0)
        
        local Health = PlayerName:Clone()
        Health.Parent = BillboardUi
        Health.Font = Enum.Font.Gotham
        Health.Position = UDim2.new(0,0,.6,0)
        
        ESP.Characters[char] = {PlayerName,Distance,Health,BillboardUi}
    --end
end
ESP.Render = function()
    
    for i,v in pairs(ESP.Characters) do
        pcall(function()
            local shrink = ((((game.Workspace.CurrentCamera.CFrame.p) - i.HumanoidRootPart.Position).Magnitude)/1500)+1
            v[2].Text = math.floor(((game.Workspace.CurrentCamera.CFrame.p) - i.HumanoidRootPart.Position).Magnitude+.5)
            v[3].Text = math.floor(i.Humanoid.Health).."/"..i.Humanoid.MaxHealth.." - "..math.floor(((i.Humanoid.Health/i.Humanoid.MaxHealth)*100)+.5).."%"
            v[3].TextColor3 = Color3.fromHSV((i.Humanoid.Health/i.Humanoid.MaxHealth) * (80/255),1,1)
            if not Spotted[i] then
                --v[1].TextTransparency = lerp(v[1].TextTransparency,.6,0.05)
            else
                --v[1].TextTransparency = lerp(v[1].TextTransparency,0,0.1)
            end
            v[1].TextColor3 = (game:GetService("Players"):GetPlayerFromCharacter(i)).TeamColor.Color
            --v[1].TextStrokeTransparency = 1-((1-v[1].TextTransparency)/2)
            --v[2].TextTransparency = v[1].TextTransparency
            --v[2].TextStrokeTransparency = v[1].TextStrokeTransparency
            --v[3].TextTransparency = v[1].TextTransparency
            --v[3].TextStrokeTransparency = v[1].TextStrokeTransparency
            v[4].Size = UDim2.new(3,60/shrink,1,30/shrink)
            v[1].Visible = ESP.Visible
            v[2].Visible = ESP.Visible
            v[3].Visible = ESP.Visible
            v[4].Parent = MainParent
            
        end)
    end
    
end
 
local Perspective = {}
Perspective.CalculatePoint = function(position,cam)
    local vector, onScreen = cam:WorldToScreenPoint(position)
    return Vector2.new(vector.X, vector.Y),vector.Z,onScreen
end
 
local Draw = {UsedLines = {}}
Draw.Line = function(a,b,line,width)
    if a and b then 
        local lerp = a:Lerp(b,.5)
        local Magnitude = (a-b).Magnitude
        line.AnchorPoint = Vector2.new(0.5,.5)
        line.Position = UDim2.new(lerp.X/game.Workspace.CurrentCamera.ViewportSize.X,0,(lerp.Y/(game.Workspace.CurrentCamera.ViewportSize.Y-36)),36/2)
        line.Size = UDim2.new(Magnitude/line.Parent.AbsoluteSize.X,0,0,width)
        line.Rotation = math.deg(math.atan2(a.y - b.y, a.x - b.x))
    else
        line:Destroy()
    end
end
 
 
Draw.Character = function(player,lines,cam)
    local Motors = CharacterMotors[player]
    if not Motors then
        Motors = CharacterMotors.GetMotors(player)
    end
 
    for i,v in pairs(Motors) do
        if v[1] and v[2] then
            local FoundLine = nil
            for i,v in pairs(lines:GetChildren()) do
                if Draw.UsedLines[v] == nil then
                    Draw.UsedLines[v] = true
                    FoundLine = v
                    break
                end
            end
            local a2d,az,screen = Perspective.CalculatePoint(v[1].Position,cam)
            local b2d,bz,screen2
            local v2pos = v[2].Position
            Spotted[player] = false
            if v[2] == "Camera" then
                local ray = Ray.new(v[1].Position, (cam.CFrame.p - v[1].Position).unit * (cam.CFrame.p - v[1].Position).Magnitude)
                local part, position = workspace:FindPartOnRayWithIgnoreList(ray, {game:GetService("Players").LocalPlayer.Character,player}, false, true)
                if part then
                    screen = false
                    screen2 = false
                else
                    Spotted[player] = true
                    b2d,bz,screen2 = Vector2.new(cam.ViewportSize.x/2, cam.ViewportSize.y),0,true   
                    screen = true
                end
                v2pos = v[1].Position
            else
                b2d,bz,screen2 = Perspective.CalculatePoint(v[2].Position,cam)  
            end
            if screen and screen2 and (v[1].Position-v2pos).Magnitude <= 5 then
                if not FoundLine then
                    FoundLine = Instance.new("Frame")
                    FoundLine.BackgroundColor3 = Color3.new(1,1,1)
                    FoundLine.BackgroundTransparency = .25
                    FoundLine.BorderSizePixel = 0
                    FoundLine.Parent = lines    
                    Draw.UsedLines[FoundLine] = true
                end
                if screen then
                    Draw.Line(a2d,b2d,FoundLine,1)
                else
                    Draw.Line(b2d,a2d,FoundLine,1)
                end
            elseif FoundLine then
                FoundLine:Destroy()
            end
        end
    end
end
 
Draw.ResetLines = function(lines)
    for i,v in pairs(lines:GetChildren()) do
        if Draw.UsedLines[v] == nil then
            v:Destroy()
        end
    end
    Draw.UsedLines = {}
end
 
local AddPlayer = function(plr)
    coroutine.resume(coroutine.create(function()
        if plr ~= game:GetService("Players").LocalPlayer  then
            if plr.Character then
                ESP.Init(plr.Character,plr)
            end
            plr.CharacterAdded:Connect(function(cha)
                ESP.Init(cha,plr)
            end)
        end
    end))
end
 
for i,v in pairs(game:GetService("Players"):GetChildren()) do
    AddPlayer(v)
end
game:GetService("Players").PlayerAdded:Connect(function(v)
    AddPlayer(v)
end)
game:GetService("UserInputService").InputBegan:connect(function(inputObject)
    if inputObject.KeyCode == ToggleKey then
        ESP.Visible = not ESP.Visible
    end
end)
game:GetService("RunService").RenderStepped:Connect(function()
    ESP.Render()
    for i,v in pairs(game:GetService("Players"):GetChildren()) do
        if v.Character and v ~= game:GetService("Players").LocalPlayer and (game:GetService("Players").LocalPlayer.Team == nil or v.Team ~= game:GetService("Players").LocalPlayer.Team) then
            if v.Character.PrimaryPart and (v.Character.PrimaryPart.Position - game.Workspace.CurrentCamera.CFrame.Position).Magnitude <= 200 then
                Draw.Character(v.Character,LineFrame,game.Workspace.CurrentCamera)
            end
        end
    end
    Draw.ResetLines(LineFrame)
end)

            RANKUP = bool
        end)
    
    





        local Farm = page1:addSection("Infinite Jump")
        local RANKUP
        Farm:addToggle("Inf Jump", nil, function(bool)
            game:GetService("UserInputService").JumpRequest:connect(function()
                game:GetService"Players".LocalPlayer.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")      
            end)
            RANKUP = bool
        end)
    




        local Farm = page1:addSection("Fly with (E)")
        local RANKUP
        Farm:addToggle("Fly (E)", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/7rXZ9VNc", true))()
            RANKUP = bool
        end)
    

        local Farm = page1:addSection("Noclip with (R)")
        local RANKUP
        Farm:addToggle("Noclip (R)", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/2pwTjwS4", true))()
            RANKUP = bool
        end)


        local Farm = page1:addSection("Alt Delet")
        local RANKUP
        Farm:addToggle("Alt Delet", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/DThr62Cn", true))()	
            RANKUP = bool
        end)


        local Farm = page1:addSection("Ctrl Teleport")
        local RANKUP
        Farm:addToggle("Ctrl Teleport", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/gp9y2Wht", true))()
            RANKUP = bool
        end)





        local Farm = page1:addSection("God Mode")
        local RANKUP
        Farm:addToggle("God Mode", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/MSZAznxp", true))()
            RANKUP = bool
        end)



        local Farm = page1:addSection("Anti-Afk")
        local RANKUP
        Farm:addToggle("Anti-Afk", nil, function(bool)
            wait(0.5)local ba=Instance.new("ScreenGui")
    local ca=Instance.new("TextLabel")local da=Instance.new("Frame")
    local _b=Instance.new("TextLabel")local ab=Instance.new("TextLabel")ba.Parent=game.CoreGui
    ba.ZIndexBehavior=Enum.ZIndexBehavior.Sibling;ca.Parent=ba;ca.Active=true
    ca.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ca.Draggable=true
    ca.Position=UDim2.new(0.698610067,0,0.098096624,0)ca.Size=UDim2.new(0,304,0,52)
    ca.Font=Enum.Font.SourceSansSemibold;ca.Text="Anti Afk Kick Script"ca.TextColor3=Color3.new(0,1,1)
    ca.TextSize=22;da.Parent=ca
    da.BackgroundColor3=Color3.new(0.196078,0.196078,0.196078)da.Position=UDim2.new(0,0,1.0192306,0)
    da.Size=UDim2.new(0,304,0,107)_b.Parent=da
    _b.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)_b.Position=UDim2.new(0,0,0.800455689,0)
    _b.Size=UDim2.new(0,304,0,21)_b.Font=Enum.Font.Arial;_b.Text="Made by: RealLeon#0005"
    _b.TextColor3=Color3.new(1,1,1)_b.TextSize=20;ab.Parent=da
    ab.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ab.Position=UDim2.new(0,0,0.158377379,0)
    ab.Size=UDim2.new(0,304,0,44)ab.Font=Enum.Font.ArialBold;ab.Text="Status: Script Started"
    ab.TextColor3=Color3.new(1,1,1)ab.TextSize=20;local bb=game:service'VirtualUser'
    game:service'Players'.LocalPlayer.Idled:connect(function()
    bb:CaptureController()bb:ClickButton2(Vector2.new())
    ab.Text="You went idle and ROBLOX tried to kick you but we reflected it!"wait(2)ab.Text="Script Re-Enabled"end)
            RANKUP = bool
        end)




        local page2 = venyx:addPage("AutoFarm", 5012544693)
        local Farm = page2:addSection("AutoFarm Money")




        
    
    
    
    





        local RANKUP
        Farm:addToggle("AutoFarm Money", nil, function(bool)
            loadstring(game:HttpGet("https://raw.githubusercontent.com/xaxaxaxaxaxaxaxaxa/Bypasses/main/Da-Hood", true))()

if not getgenv().Settings then 
    getgenv().Settings = {
        Enabled = true, -- Dont mess with this, just rejoin the game to turn the autofarm off
        TeleportSpeed = 5, -- The Lower the Faster, the Higher the Slower
    }
end

local Players, RService, TweenService = game:GetService"Players", game:GetService"RunService", game:GetService"TweenService";
local Client, CF, INew, Vec3 = Players.LocalPlayer, CFrame.new, Instance.new, Vector3.new;

local Path = workspace:WaitForChild"Ignored".Drop

function TweenTo(Part, Speed)
    local ClientPart = Client.Character:FindFirstChild"HumanoidRootPart";
    local TweenInformation = TweenService:Create(ClientPart, TweenInfo.new(
        Speed, Enum.EasingStyle.Sine, Enum.EasingDirection.InOut), {CFrame = Part}
    )
    
    if ClientPart and Part then 
        TweenInformation:Play()
    end
end

function FarmMoney()
    local SitDetection = Client.Character:WaitForChild"Humanoid".Sit
    
    while RService.RenderStepped:Wait() and getgenv().Settings.Enabled == true do 
        if SitDetection == true then 
            SitDetection = false 
        end
        
        for _, v in next, Path:GetChildren() do 
            if v and v:IsA"Part" and v.Name == "MoneyDrop" then 
                local FoundMoney = v
                local ClickDetector = FoundMoney:FindFirstChild"ClickDetector";
                local ClientPart = Client.Character:WaitForChild"HumanoidRootPart";
                
                if ClickDetector and ClientPart then 
                    TweenTo(CF(FoundMoney.Position), getgenv().Settings.TeleportSpeed)
                    
                    repeat RService.RenderStepped:Wait()
                    until ((ClientPart.Position - FoundMoney.Position).Magnitude <= ClickDetector.MaxActivationDistance)
                    
                    fireclickdetector(ClickDetector)
                end
            end
        end
    end
end
FarmMoney()

do 
    for _, v in next, workspace:GetDescendants() do 
        if v and v:IsA"Seat" then 
            v:Destroy()
        end
    end
end

Client.CharacterAdded:Connect(function()
    Client.Character:WaitForChild"Humanoid"
    
    FarmMoney()
end)

workspace.DescendantAdded:Connect(function(Descendant)
    if Descendant and Descendant:IsA"Seat" then 
        Descendant:Destroy()
    end
end)

Path.ChildAdded:Connect(function()
    FarmMoney()
end)
            RANKUP = bool
        end)

        
        
















        local Farm = page2:addSection("Auto Rob")
        local RANKUP
    Farm:addToggle("Auto Rob", nil, function(bool)
        
repeat
    wait()
until game:IsLoaded()
local gm = getrawmetatable(game)
setreadonly(gm, false)
local namecall = gm.__namecall
gm.__namecall =
    newcclosure(
    function(self, ...)
        local args = {...}
        if not checkcaller() and getnamecallmethod() == "FireServer" and tostring(self) == "MainEvent" then
            if tostring(getcallingscript()) ~= "Framework" then
                return
            end
        end
        if not checkcaller() and getnamecallmethod() == "Kick" then
            return
        end
        return namecall(self, unpack(args))
    end
)

local LocalPlayer = game:GetService("Players").LocalPlayer

function gettarget()
    local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:wait()
    local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
    local maxdistance = math.huge
    local target
    for i, v in pairs(game:GetService("Workspace").Cashiers:GetChildren()) do
        if v:FindFirstChild("Head") and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
            local distance = (HumanoidRootPart.Position - v.Head.Position).magnitude
            if distance < maxdistance then
                target = v
                maxdistance = distance
            end
        end
    end
    return target
end

for i, v in pairs(workspace:GetDescendants()) do
    if v:IsA("Seat") then
        v:Destroy()
    end
end

print("Amnesia's Da Hood Farm")

shared.MoneyFarm = true -- Just execute shared.MoneyFarm = false to stop farming

while shared.MoneyFarm do
    wait()
    local Target = gettarget()
    repeat
        wait()
        pcall(
            function()
                local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:wait()
                local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
                local Combat = LocalPlayer.Backpack:FindFirstChild("Combat") or Character:FindFirstChild("Combat")
                if not Combat then
                    Character:FindFirstChild("Humanoid").Health = 0
                    return
                end
                HumanoidRootPart.CFrame = Target.Head.CFrame * CFrame.new(0, -2.5, 3)
                Combat.Parent = Character
                Combat:Activate()
            end
        )
    until not Target or Target.Humanoid.Health < 0
    for i, v in pairs(game:GetService("Workspace").Ignored.Drop:GetDescendants()) do
        if v:IsA("ClickDetector") and v.Parent and v.Parent.Name:find("Money") then
            local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:wait()
            local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
            if (v.Parent.Position - HumanoidRootPart.Position).magnitude <= 18 then
                repeat
                    wait()
                    fireclickdetector(v)
                until not v or not v.Parent.Parent
            end
        end
    end
    wait(1)
end
		RANKUP = bool
    end)


    local RANKUP
    Farm:addToggle("Auto Rob Off", nil, function(bool)
        
repeat
    wait()
until game:IsLoaded()
local gm = getrawmetatable(game)
setreadonly(gm, false)
local namecall = gm.__namecall
gm.__namecall =
    newcclosure(
    function(self, ...)
        local args = {...}
        if not checkcaller() and getnamecallmethod() == "FireServer" and tostring(self) == "MainEvent" then
            if tostring(getcallingscript()) ~= "Framework" then
                return
            end
        end
        if not checkcaller() and getnamecallmethod() == "Kick" then
            return
        end
        return namecall(self, unpack(args))
    end
)

local LocalPlayer = game:GetService("Players").LocalPlayer

function gettarget()
    local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:wait()
    local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
    local maxdistance = math.huge
    local target
    for i, v in pairs(game:GetService("Workspace").Cashiers:GetChildren()) do
        if v:FindFirstChild("Head") and v:FindFirstChild("Humanoid") and v.Humanoid.Health > 0 then
            local distance = (HumanoidRootPart.Position - v.Head.Position).magnitude
            if distance < maxdistance then
                target = v
                maxdistance = distance
            end
        end
    end
    return target
end

for i, v in pairs(workspace:GetDescendants()) do
    if v:IsA("Seat") then
        v:Destroy()
    end
end

print("Da Hood Farm")

shared.MoneyFarm = false -- Just execute shared.MoneyFarm = false to stop farming

while shared.MoneyFarm do
    wait()
    local Target = gettarget()
    repeat
        wait()
        pcall(
            function()
                local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:wait()
                local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
                local Combat = LocalPlayer.Backpack:FindFirstChild("Combat") or Character:FindFirstChild("Combat")
                if not Combat then
                    Character:FindFirstChild("Humanoid").Health = 0
                    return
                end
                HumanoidRootPart.CFrame = Target.Head.CFrame * CFrame.new(0, -2.5, 3)
                Combat.Parent = Character
                Combat:Activate()
            end
        )
    until not Target or Target.Humanoid.Health < 0
    for i, v in pairs(game:GetService("Workspace").Ignored.Drop:GetDescendants()) do
        if v:IsA("ClickDetector") and v.Parent and v.Parent.Name:find("Money") then
            local Character = LocalPlayer.Character or LocalPlayer.CharacterAdded:wait()
            local HumanoidRootPart = Character:WaitForChild("HumanoidRootPart")
            if (v.Parent.Position - HumanoidRootPart.Position).magnitude <= 18 then
                repeat
                    wait()
                    fireclickdetector(v)
                until not v or not v.Parent.Parent
            end
        end
    end
    wait(1)
end
		RANKUP = bool
    end)






        

        












        local page5 = venyx:addPage("Credits", 5012544693)
    local Discord = page5:addSection("Discord")
    
    
    Discord:addButton("Copy Discord Link", function()
        setclipboard("https://discord.gg/fvdKvh9Csm")
    end)
    local Discord = page5:addSection("Credits")
    local d = page5:addSection("Made by: Leon????#0005")
    local d = page5:addSection("Scripter: Leon????#0005")
    local d = page5:addSection("Script Developers: /NezyGhoul#8130/Yomama#0001/Leon????#0005/")
    
    -- themes
    local themes = {
    Background = Color3.fromRGB(24, 24, 24),
    Glow = Color3.fromRGB(0, 0, 0),
    Accent = Color3.fromRGB(10, 10, 10),
    LightContrast = Color3.fromRGB(20, 20, 20),
    DarkContrast = Color3.fromRGB(14, 14, 14),
    TextColor = Color3.fromRGB(255, 255, 255)
    }
    
        
    -- Theme page
    local settings = venyx:addPage("Settings", 5012544693)
    local colors = settings:addSection("Colors")
    local setting = settings:addSection("Settings")
    
    setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
    print("Activated Keybind")
    venyx:toggle()
    end, function()
    print("Changed Keybind")
    end)
    
    for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
    colors:addColorPicker(theme, color, function(color3)
    venyx:setTheme(theme, color3)
    end)
    end
    
    -- load
    venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
    end








































































    if game.PlaceId == 142823291 then
        local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
        local venyx = library.new("Vex Hub: Murder Mystery 2", 5013109572)




        local page5 = venyx:addPage("Main", 5012544693)
    local Discord = page5:addSection("Discord")
    
    
    Discord:addButton("Copy Discord Link", function()
        setclipboard("https://discord.gg/JdEK5qN7Yb")
    end)
    local Discord = page5:addSection("Credits")
    local d = page5:addSection("Leon!#0831")
    local d = page5:addSection("Leon!#0831")
    
    
    
    
    
        

local page2 = venyx:addPage("Local Player", 5012544693)
 

        
   

 local Farm = page2:addSection("Infinite Jump")
    local RANKUP
    Farm:addToggle("Inf Jump", nil, function(bool)
        game:GetService("UserInputService").JumpRequest:connect(function()
            game:GetService"Players".LocalPlayer.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")      
        end)
		RANKUP = bool
    end)





    local Farm = page2:addSection("Fly with (E)")
    local RANKUP
    Farm:addToggle("Fly (E)", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/7rXZ9VNc", true))()
		RANKUP = bool
    end)




    local Farm = page2:addSection("Noclip with (R)")
    local RANKUP
    Farm:addToggle("Noclip (R)", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/2pwTjwS4", true))()		
        RANKUP = bool
    end)






    local Farm = page2:addSection("Alt Delet")
    local RANKUP
    Farm:addToggle("Alt Delet", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/DThr62Cn", true))()		
        RANKUP = bool
    end)




    local Farm = page2:addSection("Ctrl Teleport")
    local RANKUP
    Farm:addToggle("Ctrl Teleport", nil, function(bool)
        --[[local Plr = game:GetService("Players").LocalPlayer
local Mouse = Plr:GetMouse()
 
Mouse.Button1Down:connect(function()
if not game:GetService("UserInputService"):IsKeyDown(Enum.KeyCode.LeftControl) then return end
if not Mouse.Target then return end
Plr.Character:MoveTo(Mouse.Hit.p)
end)
--]]
    game:GetService("StarterGui"):SetCore("SendNotification",{
        Title = "CTRL click tp",
        Text = "Hold Ctrl then press click to a place you want to teleport to.",
        Duration = 6,
            })
        local speed = 100 -- set this lower to make it slower
        local bodyvelocityenabled = true -- set this to false if you are getting kicked
         
        local Imput = game:GetService("UserInputService")
        local Plr = game.Players.LocalPlayer
        local Mouse = Plr:GetMouse()
         
        function To(position)
        local Chr = Plr.Character
        if Chr ~= nil then
        local ts = game:GetService("TweenService")
        local char = game.Players.LocalPlayer.Character
        local hm = char.HumanoidRootPart
        local dist = (hm.Position - Mouse.Hit.p).magnitude
        local tweenspeed = dist/tonumber(speed)
        local ti = TweenInfo.new(tonumber(tweenspeed), Enum.EasingStyle.Linear)
        local tp = {CFrame = CFrame.new(position)}
        ts:Create(hm, ti, tp):Play()
        if bodyvelocityenabled == true then
        local bv = Instance.new("BodyVelocity")
        bv.Parent = hm
        bv.MaxForce = Vector3.new(100000,100000,100000)
        bv.Velocity = Vector3.new(0,0,0)
        wait(tonumber(tweenspeed))
        bv:Destroy()
        end
        end
        end
         
        Imput.InputBegan:Connect(function(input)
           if input.UserInputType == Enum.UserInputType.MouseButton1 and Imput:IsKeyDown(Enum.KeyCode.LeftControl) then
               To(Mouse.Hit.p)
           end
        end)	
        RANKUP = bool
    end)



    local Farm = page2:addSection("God Mode")
    local RANKUP
    Farm:addToggle("God Mode", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/MSZAznxp", true))()
        RANKUP = bool
    end)





    local Farm = page2:addSection("Anti-Afk")
    local RANKUP
    Farm:addToggle("Anti-Afk", nil, function(bool)
        wait(0.5)local ba=Instance.new("ScreenGui")
        local ca=Instance.new("TextLabel")local da=Instance.new("Frame")
        local _b=Instance.new("TextLabel")local ab=Instance.new("TextLabel")ba.Parent=game.CoreGui
        ba.ZIndexBehavior=Enum.ZIndexBehavior.Sibling;ca.Parent=ba;ca.Active=true
        ca.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ca.Draggable=true
        ca.Position=UDim2.new(0.698610067,0,0.098096624,0)ca.Size=UDim2.new(0,304,0,52)
        ca.Font=Enum.Font.SourceSansSemibold;ca.Text="Anti Afk Kick Script"ca.TextColor3=Color3.new(0,1,1)
        ca.TextSize=22;da.Parent=ca
        da.BackgroundColor3=Color3.new(0.196078,0.196078,0.196078)da.Position=UDim2.new(0,0,1.0192306,0)
        da.Size=UDim2.new(0,304,0,107)_b.Parent=da
        _b.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)_b.Position=UDim2.new(0,0,0.800455689,0)
        _b.Size=UDim2.new(0,304,0,21)_b.Font=Enum.Font.Arial;_b.Text="Made by: RealLeon#0005"
        _b.TextColor3=Color3.new(1,1,1)_b.TextSize=20;ab.Parent=da
        ab.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ab.Position=UDim2.new(0,0,0.158377379,0)
        ab.Size=UDim2.new(0,304,0,44)ab.Font=Enum.Font.ArialBold;ab.Text="Status: Script Started"
        ab.TextColor3=Color3.new(1,1,1)ab.TextSize=20;local bb=game:service'VirtualUser'
        game:service'Players'.LocalPlayer.Idled:connect(function()
        bb:CaptureController()bb:ClickButton2(Vector2.new())
        ab.Text="You went idle and ROBLOX tried to kick you but we reflected it!"wait(2)ab.Text="Script Re-Enabled"end)
        RANKUP = bool
    end)



local page3 = venyx:addPage("Visual", 5012544693)
local Farm = page3:addSection("Esp")
local Esp
    Farm:addToggle("Esp", nil, function(bool)
        loadstring(game:HttpGet(('https://pastebin.com/raw/ypSsQRK6'),true))()
		Esp = bool
    end)

    local d = page3:addSection("Esp is with Gun Esp")

    

local fd = page3:addSection("Unlock Xbox Item")
                    fd:addButton("Unlock", function()
        game:GetService("ReplicatedStorage").IsXbox:FireServer()

    end)

    
    
    
    
    
  
    
    
    
    
    
    
    
    
    
   
                   
                   
                   
                   
                   
    local page8 = venyx:addPage("Teleports", 5012544693)
    
    
    

       
                    
                    
    local Title = page8:addSection("Lobby")
    Title:addButton("Teleport", function()
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-108.25495147705, 138.3260345459, 41.639129638672)
    end)

    local Test = page8:addSection("Waiting Room")
    Test:addButton("Teleport", function()
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(-108.14199829102, 140.67608642578, 83.178932189941)
    end)





    local df = page8:addSection("Map")
    df:addButton("Teleport", function()
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(413.26495361328, 46.076118469238, 36.120166778564)
    end)



-- themes
    local themes = {
    Background = Color3.fromRGB(24, 24, 24),
    Glow = Color3.fromRGB(0, 0, 0),
    Accent = Color3.fromRGB(10, 10, 10),
    LightContrast = Color3.fromRGB(20, 20, 20),
    DarkContrast = Color3.fromRGB(14, 14, 14),
    TextColor = Color3.fromRGB(255, 255, 255)
    }
    
        
    -- Theme page
    local settings = venyx:addPage("Settings", 5012544693)
    local colors = settings:addSection("Colors")
    local setting = settings:addSection("Settings")
    
    setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
    print("Activated Keybind")
    venyx:toggle()
    end, function()
    print("Changed Keybind")
    end)
    
    for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
    colors:addColorPicker(theme, color, function(color3)
    venyx:setTheme(theme, color3)
    end)
    end











-- load
    venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
    end























    
    










































    if game.PlaceId == 1240123653 then
        local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
        local venyx = library.new("Vex Hub: Zombie Attack", 5013109572)
        
        
        
        
        
        
        











        
        
        
        
        local page2 = venyx:addPage("Local Player", 5012544693)
        local Farm = page2:addSection("Local Player")

        local Farm = page2:addSection("Infinite Jump")
    local RANKUP
    Farm:addToggle("Inf Jump", nil, function(bool)
        game:GetService("UserInputService").JumpRequest:connect(function()
            game:GetService"Players".LocalPlayer.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")      
        end)
		RANKUP = bool
    end)





    local Farm = page2:addSection("Anti-Afk")
    local RANKUP
    Farm:addToggle("Anti-Afk", nil, function(bool)
        wait(0.5)local ba=Instance.new("ScreenGui")
        local ca=Instance.new("TextLabel")local da=Instance.new("Frame")
        local _b=Instance.new("TextLabel")local ab=Instance.new("TextLabel")ba.Parent=game.CoreGui
        ba.ZIndexBehavior=Enum.ZIndexBehavior.Sibling;ca.Parent=ba;ca.Active=true
        ca.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ca.Draggable=true
        ca.Position=UDim2.new(0.698610067,0,0.098096624,0)ca.Size=UDim2.new(0,304,0,52)
        ca.Font=Enum.Font.SourceSansSemibold;ca.Text="Anti Afk Kick Script"ca.TextColor3=Color3.new(0,1,1)
        ca.TextSize=22;da.Parent=ca
        da.BackgroundColor3=Color3.new(0.196078,0.196078,0.196078)da.Position=UDim2.new(0,0,1.0192306,0)
        da.Size=UDim2.new(0,304,0,107)_b.Parent=da
        _b.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)_b.Position=UDim2.new(0,0,0.800455689,0)
        _b.Size=UDim2.new(0,304,0,21)_b.Font=Enum.Font.Arial;_b.Text="Made by: RealLeon#0005"
        _b.TextColor3=Color3.new(1,1,1)_b.TextSize=20;ab.Parent=da
        ab.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ab.Position=UDim2.new(0,0,0.158377379,0)
        ab.Size=UDim2.new(0,304,0,44)ab.Font=Enum.Font.ArialBold;ab.Text="Status: Script Started"
        ab.TextColor3=Color3.new(1,1,1)ab.TextSize=20;local bb=game:service'VirtualUser'
        game:service'Players'.LocalPlayer.Idled:connect(function()
        bb:CaptureController()bb:ClickButton2(Vector2.new())
        ab.Text="You went idle and ROBLOX tried to kick you but we reflected it!"wait(2)ab.Text="Script Re-Enabled"end)
        RANKUP = bool
    end)



    local Farm = page2:addSection("Fly with (E)")
    local RANKUP
    Farm:addToggle("Fly (E)", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/7rXZ9VNc", true))()
		RANKUP = bool
    end)




    local Farm = page2:addSection("Noclip with (R)")
    local RANKUP
    Farm:addToggle("Noclip (R)", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/2pwTjwS4", true))()		
        RANKUP = bool
    end)






    local Farm = page2:addSection("Alt Delet")
    local RANKUP
    Farm:addToggle("Alt Delet", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/DThr62Cn", true))()		
        RANKUP = bool
    end)




    local Farm = page2:addSection("Ctrl Teleport")
    local RANKUP
    Farm:addToggle("Ctrl Teleport", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/gp9y2Wht", true))()	
        RANKUP = bool
    end)



    local Farm = page2:addSection("God Mode")
    local RANKUP
    Farm:addToggle("God Mode", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/MSZAznxp", true))()
        RANKUP = bool
    end)














local page3 = venyx:addPage("AutoFarm", 5012544693)
        local Farm = page3:addSection("AutoFarm")
        local RANKUP
        Farm:addToggle("AutoFarm", nil, function(bool)
            -- Equilibrium Special Zombie_Attack Auto_Farming Enjoy Heart
local groundDistance = 8
local Player = game:GetService("Players").LocalPlayer
local function getNearest()
local nearest, dist = nil, 99999
for _,v in pairs(game.Workspace.BossFolder:GetChildren()) do
if(v:FindFirstChild("Head")~=nil)then
local m =(Player.Character.Head.Position-v.Head.Position).magnitude
if(m<dist)then
dist = m
nearest = v
end
end
end
for _,v in pairs(game.Workspace.enemies:GetChildren()) do
if(v:FindFirstChild("Head")~=nil)then
local m =(Player.Character.Head.Position-v.Head.Position).magnitude
if(m<dist)then
dist = m
nearest = v
end
end
end
return nearest
end
_G.farm2 = true
Player.Chatted:Connect(function(m)
if(m==";autofarm false")then
_G.farm2 = false
elseif(m==";autofarm true")then
_G.farm2 = true
end
end)
_G.globalTarget = nil
game:GetService("RunService").RenderStepped:Connect(function()
if(_G.farm2==true)then
local target = getNearest()
if(target~=nil)then
game:GetService("Workspace").CurrentCamera.CFrame = CFrame.new(game:GetService("Workspace").CurrentCamera.CFrame.p, target.Head.Position)
Player.Character.HumanoidRootPart.CFrame = (target.HumanoidRootPart.CFrame * CFrame.new(0, groundDistance, 9))
_G.globalTarget = target
end
end
end)
spawn(function()
while wait() do
game.Players.LocalPlayer.Character.HumanoidRootPart.Velocity = Vector3.new(0,0,0)
game.Players.LocalPlayer.Character.Torso.Velocity = Vector3.new(0,0,0)
end
end)
while wait() do
if(_G.farm2==true and _G.globalTarget~=nil and _G.globalTarget:FindFirstChild("Head") and Player.Character:FindFirstChildOfClass("Tool"))then
local target = _G.globalTarget
game.ReplicatedStorage.Gun:FireServer({["Normal"] = Vector3.new(0, 0, 0), ["Direction"] = target.Head.Position, ["Name"] = Player.Character:FindFirstChildOfClass("Tool").Name, ["Hit"] = target.Head, ["Origin"] = target.Head.Position, ["Pos"] = target.Head.Position,})
wait()
end
end
            RANKUP = bool
        end)
    
    














    local page5 = venyx:addPage("Credits", 5012544693)
    local Discord = page5:addSection("Discord")
    
    
    Discord:addButton("Copy Discord Link", function()
        setclipboard("https://discord.gg/JdEK5qN7Yb")
    end)
    local Discord = page5:addSection("Credits")
    local d = page5:addSection("Made by: Leon!#0831")
    local d = page5:addSection("Scripter: Leon!#0831")
    







    -- themes
    local themes = {
        Background = Color3.fromRGB(24, 24, 24),
        Glow = Color3.fromRGB(0, 0, 0),
        Accent = Color3.fromRGB(10, 10, 10),
        LightContrast = Color3.fromRGB(20, 20, 20),
        DarkContrast = Color3.fromRGB(14, 14, 14),
        TextColor = Color3.fromRGB(255, 255, 255)
        }
        
            
        -- Theme page
        local settings = venyx:addPage("Settings", 5012544693)
        local colors = settings:addSection("Colors")
        local setting = settings:addSection("Settings")
        
        setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
        print("Activated Keybind")
        venyx:toggle()
        end, function()
        print("Changed Keybind")
        end)
        
        for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
        colors:addColorPicker(theme, color, function(color3)
        venyx:setTheme(theme, color3)
        end)
        end
    
    
    
    
    
    
    
    
    
    
    
    -- load
        venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
        end


























































        if game.PlaceId == 3956818381 then
            local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
            local venyx = library.new("Vex Hub: Ninja Legends", 5013109572)













            local page2 = venyx:addPage("Local Player", 5012544693)
        local Farm = page2:addSection("Local Player")

        local Farm = page2:addSection("Infinite Jump")
    local RANKUP
    Farm:addToggle("Inf Jump", nil, function(bool)
        game:GetService("UserInputService").JumpRequest:connect(function()
            game:GetService"Players".LocalPlayer.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")      
        end)
		RANKUP = bool
    end)





    local Farm = page2:addSection("Anti-Afk")
    local RANKUP
    Farm:addToggle("Anti-Afk", nil, function(bool)
        wait(0.5)local ba=Instance.new("ScreenGui")
        local ca=Instance.new("TextLabel")local da=Instance.new("Frame")
        local _b=Instance.new("TextLabel")local ab=Instance.new("TextLabel")ba.Parent=game.CoreGui
        ba.ZIndexBehavior=Enum.ZIndexBehavior.Sibling;ca.Parent=ba;ca.Active=true
        ca.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ca.Draggable=true
        ca.Position=UDim2.new(0.698610067,0,0.098096624,0)ca.Size=UDim2.new(0,304,0,52)
        ca.Font=Enum.Font.SourceSansSemibold;ca.Text="Anti Afk Kick Script"ca.TextColor3=Color3.new(0,1,1)
        ca.TextSize=22;da.Parent=ca
        da.BackgroundColor3=Color3.new(0.196078,0.196078,0.196078)da.Position=UDim2.new(0,0,1.0192306,0)
        da.Size=UDim2.new(0,304,0,107)_b.Parent=da
        _b.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)_b.Position=UDim2.new(0,0,0.800455689,0)
        _b.Size=UDim2.new(0,304,0,21)_b.Font=Enum.Font.Arial;_b.Text="Made by: RealLeon#0005"
        _b.TextColor3=Color3.new(1,1,1)_b.TextSize=20;ab.Parent=da
        ab.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ab.Position=UDim2.new(0,0,0.158377379,0)
        ab.Size=UDim2.new(0,304,0,44)ab.Font=Enum.Font.ArialBold;ab.Text="Status: Script Started"
        ab.TextColor3=Color3.new(1,1,1)ab.TextSize=20;local bb=game:service'VirtualUser'
        game:service'Players'.LocalPlayer.Idled:connect(function()
        bb:CaptureController()bb:ClickButton2(Vector2.new())
        ab.Text="You went idle and ROBLOX tried to kick you but we reflected it!"wait(2)ab.Text="Script Re-Enabled"end)
        RANKUP = bool
    end)



    local Farm = page2:addSection("Fly with (E)")
    local RANKUP
    Farm:addToggle("Fly (E)", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/7rXZ9VNc", true))()
		RANKUP = bool
    end)




    local Farm = page2:addSection("Noclip with (R)")
    local RANKUP
    Farm:addToggle("Noclip (R)", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/2pwTjwS4", true))()		
        RANKUP = bool
    end)






    local Farm = page2:addSection("Alt Delete")
    local RANKUP
    Farm:addToggle("Alt Delet", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/DThr62Cn", true))()		
        RANKUP = bool
    end)




    local Farm = page2:addSection("Ctrl Teleport")
    local RANKUP
    Farm:addToggle("Ctrl Teleport", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/gp9y2Wht", true))()	
        RANKUP = bool
    end)



    local Farm = page2:addSection("God Mode")
    local RANKUP
    Farm:addToggle("God Mode", nil, function(bool)
        loadstring(game:HttpGet("https://pastebin.com/raw/MSZAznxp", true))()
        RANKUP = bool
    end)



















    local page7 = venyx:addPage("AutoFarm", 5012544693)
    


local Farm = page7:addSection("AutoFarm")
local RANKUP
    Farm:addToggle("Auto Swing", nil, function(bool)
        _G.swing = true
while _G.swing do
wait()
game.Players.LocalPlayer.ninjaEvent:FireServer("swingKatana")
end
		RANKUP = bool
    end)
    
    
    
    local Farm = page7:addSection("Auto Rank")
    local RANKUP
    Farm:addToggle("Auto Rank", nil, function(bool)
        _G.sword = true
while _G.sword do
wait()
--This script was generated by Hydroxide
local oh1 = "buyRank"
local oh2 = game:GetService("ReplicatedStorage").Ranks.Ground:GetChildren()
for i = 1,#oh2 do
game:GetService("Players").LocalPlayer.ninjaEvent:FireServer(oh1, oh2[i].Name)
end
end
		RANKUP = bool
    end)






    local Farm = page7:addSection("Auto buy Skills")
    local RANKUP
    Farm:addToggle("Auto Skills", nil, function(bool)
        _G.sword = true
while _G.sword do
wait()
--This script was generated by Hydroxide
local oh1 = "buyAllSkills"
local oh2 = {"Ground", "Astral Island", "Dragon Legend Island"}
for i = 1, #oh2 do
    game:GetService("Players").LocalPlayer.ninjaEvent:FireServer(oh1, oh2[i])
end
end
		RANKUP = bool
    end)






    local Farm = page7:addSection("Auto buy belt")
    local RANKUP
    Farm:addToggle("Auto buy belt", nil, function(bool)
        _G.sword = true
while _G.sword do
wait()
--This script was generated by Hydroxide
local oh1 = "buyAllBelts"
local oh2 = {"Ground", "Astral Island", "Golden Master Island"}
for i = 1, #oh2 do
    game:GetService("Players").LocalPlayer.ninjaEvent:FireServer(oh1, oh2[i])
end
end
		RANKUP = bool
    end)











    local Farm = page7:addSection("Auto buy Swords")
    local RANKUP
    Farm:addToggle("Auto buy Swords", nil, function(bool)
        _G.sword = true
while _G.sword do
wait()
--This script was generated by Hydroxide
local oh1 = "buyAllSwords"
local oh2 = {"Ground", "Astral Island", "Dragon Legend Island"}
for i = 1, #oh2 do
    game:GetService("Players").LocalPlayer.ninjaEvent:FireServer(oh1, oh2[i])
end
end
		RANKUP = bool
    end)




    
    





local Farm = page7:addSection("Auto Sell")

local RANKUP
    Farm:addToggle("Auto Sell", nil, function(bool)
        _G.Condition = true -- true turns it on, false turns it off
        while _G.Condition == true do
        wait(1)
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(84.2945251, 87069.0391, 123.435143)
        wait()
      end
		RANKUP = bool
    end)




local page9 = venyx:addPage("Elements", 5012544693)


local Title = page9:addSection("Unlock Frost Element")
     Title:addButton("Unlock Frost Element", function()
        game.ReplicatedStorage.rEvents.elementMasteryEvent:FireServer("Frost")
    end)



local fd = page9:addSection("Unlock Lightning Element")
     fd:addButton("Unlock Lightning Element", function()
    game.ReplicatedStorage.rEvents.elementMasteryEvent:FireServer("Lightning")
    end)

  
  
    
local fds = page9:addSection("Unlock Inferno Element")
     fds:addButton("Unlock Inferno Element", function()
       game.ReplicatedStorage.rEvents.elementMasteryEvent:FireServer("Inferno")
    end)
    




    local Electral = page9:addSection("Unlock Electral Chaos Element")
    Electral:addButton("Unlock Electral Chaos Element", function()
        game.ReplicatedStorage.rEvents.elementMasteryEvent:FireServer("Electral Chaos")
    end)






    local Masterful = page9:addSection("Unlock Masterful Wrath Element")
    Masterful:addButton("Unlock Masterful Wrath Element", function()
        game.ReplicatedStorage.rEvents.elementMasteryEvent:FireServer("Masterful Wrath")
    end)






    local Shadow = page9:addSection("Unlock Shadow Charge Element")
    Shadow:addButton("Unlock Shadow Charge Element", function()
        game.ReplicatedStorage.rEvents.elementMasteryEvent:FireServer("Shadow Charge")
    end)











    local Shadowfire = page9:addSection("Unlock Shadowfire Element")
    Shadowfire:addButton("Unlock Shadowfire Element", function()
        game.ReplicatedStorage.rEvents.elementMasteryEvent:FireServer("Shadowfire")
    end)









    local Eternity = page9:addSection("Unlock Eternity Storm Element")
    Eternity:addButton("Unlock Eternity Storm Element", function()
        game.ReplicatedStorage.rEvents.elementMasteryEvent:FireServer("Eternity Storm")
    end)












    local page5 = venyx:addPage("Credits", 5012544693)
    local Discord = page5:addSection("Discord")
    
    
    Discord:addButton("Copy Discord Link", function()
        setclipboard("https://discord.gg/JdEK5qN7Yb")
    end)
    local Discord = page5:addSection("Credits")
    local d = page5:addSection("Made by: Leon!#0831")
    local d = page5:addSection("Scripter: Leon!#0831")







    -- themes
    local themes = {
        Background = Color3.fromRGB(24, 24, 24),
        Glow = Color3.fromRGB(0, 0, 0),
        Accent = Color3.fromRGB(10, 10, 10),
        LightContrast = Color3.fromRGB(20, 20, 20),
        DarkContrast = Color3.fromRGB(14, 14, 14),
        TextColor = Color3.fromRGB(255, 255, 255)
        }
        
            
        -- Theme page
        local settings = venyx:addPage("Settings", 5012544693)
        local colors = settings:addSection("Colors")
        local setting = settings:addSection("Settings")
        
        setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
        print("Activated Keybind")
        venyx:toggle()
        end, function()
        print("Changed Keybind")
        end)
        
        for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
        colors:addColorPicker(theme, color, function(color3)
        venyx:setTheme(theme, color3)
        end)
        end
    
    
    
    
    
    
    
    
    
    
    
    -- load
        venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
        end












































































































        if game.PlaceId == 1224212277 then
            local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
            local venyx = library.new("Vex Hub: Mad City", 5013109572)













            local page3 = venyx:addPage("Main", 5012544693)
            
local Farm = page3:addSection("Farm Xp")
local RANKUP
    Farm:addToggle("Farm Xp", nil, function(bool)
        game:GetService("ReplicatedStorage").RemoteFunction:InvokeServer("SetTeam", "Police")
wait(.75)
game:GetService("RunService").RenderStepped:Connect(function()
for i,v in pairs(game:GetService("Players").LocalPlayer.Backpack:GetChildren()) do
   if v.Name == "Handcuffs" then v.Parent = game:GetService("Players").LocalPlayer.Character
   end
end
game:GetService("ReplicatedStorage").Event:FireServer("Eject", game:GetService("Players").LocalPlayer)
end)
		RANKUP = bool
    end)






    local Farm = page3:addSection("Drive on Water")
    local RANKUP
    Farm:addToggle("Drive on Water", nil, function(bool)
        game:GetService("Workspace").ObjectSelection[game.Players.LocalPlayer.Name .. "'s Vehicle"].CarChassis.HoverMode.Value = true
		RANKUP = bool
    end)



    local Farm = page3:addSection("Walk on Water")
    local RANKUP
    Farm:addToggle("Walk on Water", nil, function(bool)
        for k,v in pairs(game:GetService("Workspace").Water:GetChildren()) do
            v.CanCollide = true
            v.Anchored = true
            v.Material = "Ice"
         end
		RANKUP = bool
    end)

            

    local Farm = page3:addSection("Infinite Nitro")
    local RANKUP
    Farm:addToggle("Infinite Nitro", nil, function(bool)
        game:GetService("RunService").RenderStepped:Connect(function()
            if workspace.ObjectSelection:FindFirstChild(game.Players.LocalPlayer.Name.."'s Vehicle") then
            pcall(function()workspace.ObjectSelection[game.Players.LocalPlayer.Name.."'s Vehicle"].CarChassis.Boost.Value = 20;end)
            end
            end)
		RANKUP = bool
    end)



    local Farm = page3:addSection("Crash Server")
    local RANKUP
    Farm:addToggle("Crash Server", nil, function(bool)
        loadstring(game:HttpGet(('https://raw.githubusercontent.com/Cesare0328/my-scripts/main/Mad%20Shitty%20instant%20crash%20server'),true))()
		RANKUP = bool
    end)




    local dfdf = page3:addSection("Rejoin Server")
    
    
    dfdf:addButton("Rejoin", function()
        game:GetService("TeleportService"):Teleport(game.PlaceId)
    end)







            



    local page4 = venyx:addPage("Weapons", 5012544693)
    local Farm = page4:addSection("Inf Ammo")
    local RANKUP
    Farm:addToggle("Inf Ammo", nil, function(bool)
        local function main(v)
            if v.Name == "RifleScript" or v.Name == "ShotgunScript" or v.Name == "PistolScript" or v.Name == "TazerScript" or v.Name == "PowerScript" then
                local a = getsenv(v)
                if debug.getupvalues(a.Reload) then
                    debug.setupvalue(a.Reload, 2, math.huge)
                end
            end
         end
         
         for _, v in pairs(game.Players.LocalPlayer.Backpack:GetDescendants()) do main(v) end
         for _, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do main(v) end
		RANKUP = bool
    end)








            
            
            
            local page2 = venyx:addPage("Local Player", 5012544693)
            local Farm = page2:addSection("Local Player")
    
            local Farm = page2:addSection("Infinite Jump")
        local RANKUP
        Farm:addToggle("Inf Jump", nil, function(bool)
            game:GetService("UserInputService").JumpRequest:connect(function()
                game:GetService"Players".LocalPlayer.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")      
            end)
            RANKUP = bool
        end)
    
    
    
    
    
        local Farm = page2:addSection("Anti-Afk")
        local RANKUP
        Farm:addToggle("Anti-Afk", nil, function(bool)
            wait(0.5)local ba=Instance.new("ScreenGui")
            local ca=Instance.new("TextLabel")local da=Instance.new("Frame")
            local _b=Instance.new("TextLabel")local ab=Instance.new("TextLabel")ba.Parent=game.CoreGui
            ba.ZIndexBehavior=Enum.ZIndexBehavior.Sibling;ca.Parent=ba;ca.Active=true
            ca.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ca.Draggable=true
            ca.Position=UDim2.new(0.698610067,0,0.098096624,0)ca.Size=UDim2.new(0,304,0,52)
            ca.Font=Enum.Font.SourceSansSemibold;ca.Text="Anti Afk Kick Script"ca.TextColor3=Color3.new(0,1,1)
            ca.TextSize=22;da.Parent=ca
            da.BackgroundColor3=Color3.new(0.196078,0.196078,0.196078)da.Position=UDim2.new(0,0,1.0192306,0)
            da.Size=UDim2.new(0,304,0,107)_b.Parent=da
            _b.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)_b.Position=UDim2.new(0,0,0.800455689,0)
            _b.Size=UDim2.new(0,304,0,21)_b.Font=Enum.Font.Arial;_b.Text="Made by: RealLeon#0005"
            _b.TextColor3=Color3.new(1,1,1)_b.TextSize=20;ab.Parent=da
            ab.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ab.Position=UDim2.new(0,0,0.158377379,0)
            ab.Size=UDim2.new(0,304,0,44)ab.Font=Enum.Font.ArialBold;ab.Text="Status: Script Started"
            ab.TextColor3=Color3.new(1,1,1)ab.TextSize=20;local bb=game:service'VirtualUser'
            game:service'Players'.LocalPlayer.Idled:connect(function()
            bb:CaptureController()bb:ClickButton2(Vector2.new())
            ab.Text="You went idle and ROBLOX tried to kick you but we reflected it!"wait(2)ab.Text="Script Re-Enabled"end)
            RANKUP = bool
        end)
    
    
    
        local Farm = page2:addSection("Fly with (E)")
        local RANKUP
        Farm:addToggle("Fly (E)", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/7rXZ9VNc", true))()
            RANKUP = bool
        end)
    
    



        local Title = page2:addSection("Reset Character")
    
    
        Title:addButton("Reset", function()
            game.Players.LocalPlayer.Character.Head:Remove()
        end)
    
    



    
    
        local Farm = page2:addSection("Noclip with (R)")
        local RANKUP
        Farm:addToggle("Noclip (R)", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/2pwTjwS4", true))()		
            RANKUP = bool
        end)
    
    
    



        local Farm = page2:addSection("God Mode")
        local RANKUP
        Farm:addToggle("God Mode", nil, function(bool)
            loadstring(game:HttpGet(('https://raw.githubusercontent.com/Cesare0328/my-scripts/main/Mad%20City%20GodMode'),true))()
            RANKUP = bool
        end)


    
    
    
        local Farm = page2:addSection("Alt Delete")
        local RANKUP
        Farm:addToggle("Alt Delet", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/DThr62Cn", true))()		
            RANKUP = bool
        end)
    
    
    
    
        local Farm = page2:addSection("Ctrl Teleport")
        local RANKUP
        Farm:addToggle("Ctrl Teleport", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/gp9y2Wht", true))()	
            RANKUP = bool
        end)
    
    
    
        local Farm = page2:addSection("God Mode")
        local RANKUP
        Farm:addToggle("God Mode", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/MSZAznxp", true))()
            RANKUP = bool
        end)































            local page5 = venyx:addPage("Credits", 5012544693)
            local Discord = page5:addSection("Discord")
            
            
            Discord:addButton("Copy Discord Link", function()
                setclipboard("https://discord.gg/JdEK5qN7Yb")
            end)
            local Discord = page5:addSection("Credits")
            local d = page5:addSection("Made by: Leon!#0831")
            local d = page5:addSection("Scripter: Leon!#0831")
        
        
        
        
        
        
        
            -- themes
            local themes = {
                Background = Color3.fromRGB(24, 24, 24),
                Glow = Color3.fromRGB(0, 0, 0),
                Accent = Color3.fromRGB(10, 10, 10),
                LightContrast = Color3.fromRGB(20, 20, 20),
                DarkContrast = Color3.fromRGB(14, 14, 14),
                TextColor = Color3.fromRGB(255, 255, 255)
                }
                
                    
                -- Theme page
                local settings = venyx:addPage("Settings", 5012544693)
                local colors = settings:addSection("Colors")
                local setting = settings:addSection("Settings")
                
                setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
                print("Activated Keybind")
                venyx:toggle()
                end, function()
                print("Changed Keybind")
                end)
                
                for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
                colors:addColorPicker(theme, color, function(color3)
                venyx:setTheme(theme, color3)
                end)
                end
            
            
            
            
            
            
            
            
            
            
            
            -- load
                venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
                end




















































 







               
























































            if game.PlaceId == 3101667897 then
            local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
            local venyx = library.new("Vex Hub: Legend Of Speed", 5013109572)



local page2 = venyx:addPage("AutoFarm", 5012544693)
                    local Farm = page2:addSection("Speed and Level Farm")
local RANKUP
    Farm:addToggle("AutoFarm", nil, function(bool)
        while wait(0.1) do
            local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3)
            end
             
            while wait(0.1) do
            local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3)
            end
             
            while wait(0.1) do
            local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3)
            end
             
            while wait(0.1) do
            local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3)
            end
             
            while wait(0.1) do
            local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Gem" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Yellow Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Orange Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3) local A_1 = "collectOrb" local A_2 = "Blue Orb" local A_3 = "City" local Event = game:GetService("ReplicatedStorage").rEvents.orbEvent Event:FireServer(A_1, A_2, A_3)
            end
		RANKUP = bool
    end)




    Farm:addButton("Auto Orbs And Diemonds", function()
        loadstring(game:HttpGet(('https://raw.githubusercontent.com/goodluck3646/legends-of-speed-mod/main/README.md'),true))()
        end)



        Farm:addButton("Auto Collect Orbs", function()
            getgenv().farm = true

local PlayerRoot = game.Players.LocalPlayer.Character.HumanoidRootPart

while getgenv().farm do -- you don't need ==true in most cases
  wait(0.2) -- always add a wait when using while ... do
  for i,v in pairs(game:GetService("Workspace").orbFolder.City:GetDescendants()) do
      if v.name == "TouchInterest" and v.Parent then
          firetouchinterest(v.Parent, PlayerRoot, 0)
          firetouchinterest(v.Parent, PlayerRoot, 1)
          wait()
      end
  end
end
end)












                    local page2 = venyx:addPage("Esp", 5012544693)
                    
                    local Farm1 = page2:addSection("Player Esp")
                    local RANKUP
                    Farm1:addToggle("Esp", nil, function(bool)
                        local MainParent = game.workspace.Terrain or nil
local LineFrame = Instance.new("ScreenGui",MainParent)
LineFrame.Name = "Lines"
LineFrame.Enabled = false
ToggleKey = Enum.KeyCode.F1
local Spotted = {};
local CharacterMotors = {}
CharacterMotors.GetMotors = function(char)
    local Motors = {}
        for i,v in pairs(char:GetDescendants()) do
            if v:IsA("Motor6D") then
                if v.Part0 and v.Part1 then--and v.Part0.Name ~= "HumanoidRootPart" and v.Part1.Name ~= "HumanoidRootPart" then
                    table.insert(Motors,{v.Part0,v.Part1})
                end
            end
        end
    table.insert(Motors,{char:FindFirstChild("Head") or char.PrimaryPart,"Camera"})
    CharacterMotors[char] = Motors
    return CharacterMotors[char]
end
        
lerp = function(a, b, t)
    return a + (b - a) * t
end
 
local ESP = {Characters = {}}
ESP.Visible = true
ESP.Init = function(char,plr)
    --if (game:GetService("Players").LocalPlayer.Team == nil or plr.Team ~= game:GetService("Players").LocalPlayer.Team) then
        char.DescendantAdded:Connect(function()
            CharacterMotors.GetMotors(char)
        end)
        
        local BillboardUi = Instance.new("BillboardGui")
        BillboardUi.AlwaysOnTop = true
        BillboardUi.Size = UDim2.new(3,60,1,30)
        BillboardUi.StudsOffsetWorldSpace = Vector3.new(0,-4.5,0)
        BillboardUi.Adornee = char:WaitForChild("HumanoidRootPart")
        
        local PlayerName = Instance.new("TextLabel",BillboardUi)
        PlayerName.BackgroundTransparency = 1
        PlayerName.TextColor3 = (game:GetService("Players"):GetPlayerFromCharacter(char)).TeamColor.Color
        PlayerName.Size = UDim2.new(1,0,.3,0)
        PlayerName.ZIndex = 2
        PlayerName.Text = char.Name
        PlayerName.TextScaled = true
        PlayerName.TextStrokeTransparency = .25
        PlayerName.Font = Enum.Font.GothamBold
        PlayerName.TextStrokeColor3 = Color3.fromRGB(33, 33, 33)
        
        local Distance = PlayerName:Clone()
        Distance.Parent = BillboardUi
        Distance.Font = Enum.Font.Gotham
        Distance.TextColor3 = Color3.new(1,1,1)
        Distance.Position = UDim2.new(0,0,.3,0)
        
        local Health = PlayerName:Clone()
        Health.Parent = BillboardUi
        Health.Font = Enum.Font.Gotham
        Health.Position = UDim2.new(0,0,.6,0)
        
        ESP.Characters[char] = {PlayerName,Distance,Health,BillboardUi}
    --end
end
ESP.Render = function()
    
    for i,v in pairs(ESP.Characters) do
        pcall(function()
            local shrink = ((((game.Workspace.CurrentCamera.CFrame.p) - i.HumanoidRootPart.Position).Magnitude)/1500)+1
            v[2].Text = math.floor(((game.Workspace.CurrentCamera.CFrame.p) - i.HumanoidRootPart.Position).Magnitude+.5)
            v[3].Text = math.floor(i.Humanoid.Health).."/"..i.Humanoid.MaxHealth.." - "..math.floor(((i.Humanoid.Health/i.Humanoid.MaxHealth)*100)+.5).."%"
            v[3].TextColor3 = Color3.fromHSV((i.Humanoid.Health/i.Humanoid.MaxHealth) * (80/255),1,1)
            if not Spotted[i] then
                --v[1].TextTransparency = lerp(v[1].TextTransparency,.6,0.05)
            else
                --v[1].TextTransparency = lerp(v[1].TextTransparency,0,0.1)
            end
            v[1].TextColor3 = (game:GetService("Players"):GetPlayerFromCharacter(i)).TeamColor.Color
            --v[1].TextStrokeTransparency = 1-((1-v[1].TextTransparency)/2)
            --v[2].TextTransparency = v[1].TextTransparency
            --v[2].TextStrokeTransparency = v[1].TextStrokeTransparency
            --v[3].TextTransparency = v[1].TextTransparency
            --v[3].TextStrokeTransparency = v[1].TextStrokeTransparency
            v[4].Size = UDim2.new(3,60/shrink,1,30/shrink)
            v[1].Visible = ESP.Visible
            v[2].Visible = ESP.Visible
            v[3].Visible = ESP.Visible
            v[4].Parent = MainParent
            
        end)
    end
    
end
 
local Perspective = {}
Perspective.CalculatePoint = function(position,cam)
    local vector, onScreen = cam:WorldToScreenPoint(position)
    return Vector2.new(vector.X, vector.Y),vector.Z,onScreen
end
 
local Draw = {UsedLines = {}}
Draw.Line = function(a,b,line,width)
    if a and b then 
        local lerp = a:Lerp(b,.5)
        local Magnitude = (a-b).Magnitude
        line.AnchorPoint = Vector2.new(0.5,.5)
        line.Position = UDim2.new(lerp.X/game.Workspace.CurrentCamera.ViewportSize.X,0,(lerp.Y/(game.Workspace.CurrentCamera.ViewportSize.Y-36)),36/2)
        line.Size = UDim2.new(Magnitude/line.Parent.AbsoluteSize.X,0,0,width)
        line.Rotation = math.deg(math.atan2(a.y - b.y, a.x - b.x))
    else
        line:Destroy()
    end
end
 
 
Draw.Character = function(player,lines,cam)
    local Motors = CharacterMotors[player]
    if not Motors then
        Motors = CharacterMotors.GetMotors(player)
    end
 
    for i,v in pairs(Motors) do
        if v[1] and v[2] then
            local FoundLine = nil
            for i,v in pairs(lines:GetChildren()) do
                if Draw.UsedLines[v] == nil then
                    Draw.UsedLines[v] = true
                    FoundLine = v
                    break
                end
            end
            local a2d,az,screen = Perspective.CalculatePoint(v[1].Position,cam)
            local b2d,bz,screen2
            local v2pos = v[2].Position
            Spotted[player] = false
            if v[2] == "Camera" then
                local ray = Ray.new(v[1].Position, (cam.CFrame.p - v[1].Position).unit * (cam.CFrame.p - v[1].Position).Magnitude)
                local part, position = workspace:FindPartOnRayWithIgnoreList(ray, {game:GetService("Players").LocalPlayer.Character,player}, false, true)
                if part then
                    screen = false
                    screen2 = false
                else
                    Spotted[player] = true
                    b2d,bz,screen2 = Vector2.new(cam.ViewportSize.x/2, cam.ViewportSize.y),0,true   
                    screen = true
                end
                v2pos = v[1].Position
            else
                b2d,bz,screen2 = Perspective.CalculatePoint(v[2].Position,cam)  
            end
            if screen and screen2 and (v[1].Position-v2pos).Magnitude <= 5 then
                if not FoundLine then
                    FoundLine = Instance.new("Frame")
                    FoundLine.BackgroundColor3 = Color3.new(1,1,1)
                    FoundLine.BackgroundTransparency = .25
                    FoundLine.BorderSizePixel = 0
                    FoundLine.Parent = lines    
                    Draw.UsedLines[FoundLine] = true
                end
                if screen then
                    Draw.Line(a2d,b2d,FoundLine,1)
                else
                    Draw.Line(b2d,a2d,FoundLine,1)
                end
            elseif FoundLine then
                FoundLine:Destroy()
            end
        end
    end
end
 
Draw.ResetLines = function(lines)
    for i,v in pairs(lines:GetChildren()) do
        if Draw.UsedLines[v] == nil then
            v:Destroy()
        end
    end
    Draw.UsedLines = {}
end
 
local AddPlayer = function(plr)
    coroutine.resume(coroutine.create(function()
        if plr ~= game:GetService("Players").LocalPlayer  then
            if plr.Character then
                ESP.Init(plr.Character,plr)
            end
            plr.CharacterAdded:Connect(function(cha)
                ESP.Init(cha,plr)
            end)
        end
    end))
end
 
for i,v in pairs(game:GetService("Players"):GetChildren()) do
    AddPlayer(v)
end
game:GetService("Players").PlayerAdded:Connect(function(v)
    AddPlayer(v)
end)
game:GetService("UserInputService").InputBegan:connect(function(inputObject)
    if inputObject.KeyCode == ToggleKey then
        ESP.Visible = not ESP.Visible
    end
end)
game:GetService("RunService").RenderStepped:Connect(function()
    ESP.Render()
    for i,v in pairs(game:GetService("Players"):GetChildren()) do
        if v.Character and v ~= game:GetService("Players").LocalPlayer and (game:GetService("Players").LocalPlayer.Team == nil or v.Team ~= game:GetService("Players").LocalPlayer.Team) then
            if v.Character.PrimaryPart and (v.Character.PrimaryPart.Position - game.Workspace.CurrentCamera.CFrame.Position).Magnitude <= 200 then
                Draw.Character(v.Character,LineFrame,game.Workspace.CurrentCamera)
            end
        end
    end
    Draw.ResetLines(LineFrame)
end)

                        RANKUP = bool
                    end)
                
                
                
                
















                    local page2 = venyx:addPage("Local Player", 5012544693)
            local Farm = page2:addSection("Local Player")
    

    
            local Farm = page2:addSection("Infinite Jump")
        local RANKUP
        Farm:addToggle("Inf Jump", nil, function(bool)
            game:GetService("UserInputService").JumpRequest:connect(function()
                game:GetService"Players".LocalPlayer.Character:FindFirstChildOfClass'Humanoid':ChangeState("Jumping")      
            end)
            RANKUP = bool
        end)
    
    
    
    
    
        local Farm = page2:addSection("Anti-Afk")
        local RANKUP
        Farm:addToggle("Anti-Afk", nil, function(bool)
            wait(0.5)local ba=Instance.new("ScreenGui")
            local ca=Instance.new("TextLabel")local da=Instance.new("Frame")
            local _b=Instance.new("TextLabel")local ab=Instance.new("TextLabel")ba.Parent=game.CoreGui
            ba.ZIndexBehavior=Enum.ZIndexBehavior.Sibling;ca.Parent=ba;ca.Active=true
            ca.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ca.Draggable=true
            ca.Position=UDim2.new(0.698610067,0,0.098096624,0)ca.Size=UDim2.new(0,304,0,52)
            ca.Font=Enum.Font.SourceSansSemibold;ca.Text="Anti Afk Kick Script"ca.TextColor3=Color3.new(0,1,1)
            ca.TextSize=22;da.Parent=ca
            da.BackgroundColor3=Color3.new(0.196078,0.196078,0.196078)da.Position=UDim2.new(0,0,1.0192306,0)
            da.Size=UDim2.new(0,304,0,107)_b.Parent=da
            _b.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)_b.Position=UDim2.new(0,0,0.800455689,0)
            _b.Size=UDim2.new(0,304,0,21)_b.Font=Enum.Font.Arial;_b.Text="Made by: RealLeon#0005"
            _b.TextColor3=Color3.new(1,1,1)_b.TextSize=20;ab.Parent=da
            ab.BackgroundColor3=Color3.new(0.176471,0.176471,0.176471)ab.Position=UDim2.new(0,0,0.158377379,0)
            ab.Size=UDim2.new(0,304,0,44)ab.Font=Enum.Font.ArialBold;ab.Text="Status: Script Started"
            ab.TextColor3=Color3.new(1,1,1)ab.TextSize=20;local bb=game:service'VirtualUser'
            game:service'Players'.LocalPlayer.Idled:connect(function()
            bb:CaptureController()bb:ClickButton2(Vector2.new())
            ab.Text="You went idle and ROBLOX tried to kick you but we reflected it!"wait(2)ab.Text="Script Re-Enabled"end)
            RANKUP = bool
        end)
    
    
    
        local Farm = page2:addSection("Fly with (E)")
        local RANKUP
        Farm:addToggle("Fly (E)", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/7rXZ9VNc", true))()
            RANKUP = bool
        end)
    
    



        local Title = page2:addSection("Reset Character")
    
    
        Title:addButton("Reset", function()
            game.Players.LocalPlayer.Character.Head:Remove()
        end)
    
    



    
    
        local Farm = page2:addSection("Noclip with (R)")
        local RANKUP
        Farm:addToggle("Noclip (R)", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/2pwTjwS4", true))()		
            RANKUP = bool
        end)
    



    
    
    
        local Farm = page2:addSection("Alt Delete")
        local RANKUP
        Farm:addToggle("Alt Delet", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/DThr62Cn", true))()		
            RANKUP = bool
        end)
    
    
    
    
        local Farm = page2:addSection("Ctrl Teleport")
        local RANKUP
        Farm:addToggle("Ctrl Teleport", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/gp9y2Wht", true))()	
            RANKUP = bool
        end)
    
    
    
        local Farm = page2:addSection("God Mode")
        local RANKUP
        Farm:addToggle("God Mode", nil, function(bool)
            loadstring(game:HttpGet("https://pastebin.com/raw/MSZAznxp", true))()
            RANKUP = bool
        end)



            local page5 = venyx:addPage("Credits", 5012544693)
            local Discord = page5:addSection("Discord")
            
            
            Discord:addButton("Copy Discord Link", function()
                setclipboard("https://discord.gg/JdEK5qN7Yb")
            end)
            local Discord = page5:addSection("Credits")
            local d = page5:addSection("Made by: Leon!#0831")
            local d = page5:addSection("Scripter: Leon!#0831")
        
        
        
        
        
        
        
            -- themes
            local themes = {
                Background = Color3.fromRGB(24, 24, 24),
                Glow = Color3.fromRGB(0, 0, 0),
                Accent = Color3.fromRGB(10, 10, 10),
                LightContrast = Color3.fromRGB(20, 20, 20),
                DarkContrast = Color3.fromRGB(14, 14, 14),
                TextColor = Color3.fromRGB(255, 255, 255)
                }
                
                    
                -- Theme page
                local settings = venyx:addPage("Settings", 5012544693)
                local colors = settings:addSection("Colors")
                local setting = settings:addSection("Settings")
                
                setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
                print("Activated Keybind")
                venyx:toggle()
                end, function()
                print("Changed Keybind")
                end)
                
                for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
                colors:addColorPicker(theme, color, function(color3)
                venyx:setTheme(theme, color3)
                end)
                end
                
            -- load
                venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
                end


































































            if game.PlaceId == 292439477 then
            local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
            local venyx = library.new("Vex Hub: Phantom Forces", 5013109572)



            local page3 = venyx:addPage("Aimbot", 5012544693)
            
            local Farm = page3:addSection("Silent Aim")
            page3:addButton("Silent Aim", function()
                -- Credits to integerisqt!
-- Have a great day!
local Players = game:GetService("Players")
local Camera = workspace.CurrentCamera
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local GameLogic, CharTable, Trajectory
for I,V in pairs(getgc(true)) do
    if type(V) == "table" then
        if rawget(V, "gammo") then
            GameLogic = V
        end
    elseif type(V) == "function" then
        if debug.getinfo(V).name == "getbodyparts" then
            CharTable = debug.getupvalue(V, 1)
        elseif debug.getinfo(V).name == "trajectory" then
            Trajectory = V
        end
    end
    if GameLogic and CharTable and Trajectory then break end
end

local function Closest()
    local Max, Close = math.huge
    for I,V in pairs(Players:GetPlayers()) do
        if V ~= LocalPlayer and V.Team ~= LocalPlayer.Team and CharTable[V] then
            local Pos, OnScreen = Camera:WorldToScreenPoint(CharTable[V].head.Position)
            if OnScreen then
                local Dist = (Vector2.new(Pos.X, Pos.Y) - Vector2.new(Mouse.X, Mouse.Y)).Magnitude
                if Dist < Max then
                    Max = Dist
                    Close = V
                end
            end
        end
    end
    return Close
end
local MT = getrawmetatable(game)
local __index = MT.__index
setreadonly(MT, false)
MT.__index = newcclosure(function(self, K)
    if not checkcaller() and GameLogic.currentgun and GameLogic.currentgun.data and (self == GameLogic.currentgun.barrel or tostring(self) == "SightMark") and K == "CFrame" then
        local CharChosen = (CharTable[Closest()] and CharTable[Closest()].head)
        if CharChosen then
            local _, Time = Trajectory(self.Position, Vector3.new(0, -workspace.Gravity, 0), CharChosen.Position, GameLogic.currentgun.data.bulletspeed)
            return CFrame.new(self.Position, CharChosen.Position + (Vector3.new(0, -workspace.Gravity / 2, 0)) * (Time ^ 2) + (CharChosen.Velocity * Time))
        end
    end
    return __index(self, K)
end)
setreadonly(MT, true)
                end)





                local page5 = venyx:addPage("Credits", 5012544693)
            local Discord = page5:addSection("Discord")
            
            
            Discord:addButton("Copy Discord Link", function()
                setclipboard("https://discord.gg/JdEK5qN7Yb")
            end)
            local Discord = page5:addSection("Credits")
            local d = page5:addSection("Made by: Leon!#0831")
            local d = page5:addSection("Scripter: Leon!#0831")
        
        
        
        
        
        
        
            -- themes
            local themes = {
                Background = Color3.fromRGB(24, 24, 24),
                Glow = Color3.fromRGB(0, 0, 0),
                Accent = Color3.fromRGB(10, 10, 10),
                LightContrast = Color3.fromRGB(20, 20, 20),
                DarkContrast = Color3.fromRGB(14, 14, 14),
                TextColor = Color3.fromRGB(255, 255, 255)
                }
                
                    
                -- Theme page
                local settings = venyx:addPage("Settings", 5012544693)
                local colors = settings:addSection("Colors")
                local setting = settings:addSection("Settings")
                
                setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
                print("Activated Keybind")
                venyx:toggle()
                end, function()
                print("Changed Keybind")
                end)
                
                for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
                colors:addColorPicker(theme, color, function(color3)
                venyx:setTheme(theme, color3)
                end)
                end
                
            -- load
                venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
                end


                






























                if game.PlaceId == 443406476 then
                    local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
                    local venyx = library.new("Vex Hub: Projekt Lazarus", 5013109572)




                 
   
local section1 = venyx:addPage("Main", 8162117119)
                     local section1 = section1:addSection("Main")



                    
                    section1:addButton("One shot Zombies", function()
                        hookfunction(gcinfo, function()
                            return math.random(200,350)
                         end)
                         
                         -- // Constants \\ --
                         -- [ Services ] --
                         local Services = setmetatable({}, {__index = function(Self, Index)
                            local NewService = game:GetService(Index)
                            if NewService then
                                Self[Index] = NewService
                            end
                            return NewService
                         end})
                         
                         -- [ LocalPlayer ] --
                         local LocalPlayer = Services.Players.LocalPlayer
                         
                         -- // Variables \\ --
                         local Connections = {
                            Weapon1 = nil;
                            Weapon2 = nil;
                            Weapon3 = nil;
                            Backpack = nil;
                         }
                         
                         local RoundNumber = workspace.RoundNum
                         
                         -- // Functions \\ --
                         local function CharacterAdded(Character)
                            local Backpack = LocalPlayer:WaitForChild('Backpack')
                         
                            local function ChildAdded(Child)
                                if Child.Name == "Weapon1" or Child.Name == "Weapon2" or Child.Name == "Weapon3" then
                                    local Module = require(Child)
                                    
                                    if Connections[Child.Name] then
                                        Connections[Child.Name]:Disconnect()
                                        Connections[Child.Name] = nil
                                    end
                         
                                    Connections[Child.Name] = Services.RunService.RenderStepped:Connect(function()
                                        Module.Ammo = Module.MagSize + 1
                                        Module.StoredAmmo = Module.MaxAmmo
                                        Module.HeadShot = 150 + (RoundNumber.Value * 150)
                                        Module.TorsoShot = 150 + (RoundNumber.Value * 150)
                                        Module.LimbShot = 150 + (RoundNumber.Value * 150)
                                        Module.BulletPenetration = 10
                                    end)
                                end
                            end
                         
                            if Connections.Backpack then
                                Connections.Backpack:Disconnect()
                                Connections.Backpack = nil
                            end
                         
                            for i,v in ipairs(Backpack:GetChildren()) do
                                ChildAdded(v)
                            end
                            Connections.Backpack = Backpack.ChildAdded:Connect(ChildAdded)
                         end
                         
                         -- // Event Listeners \\ --
                         LocalPlayer.CharacterAdded:Connect(CharacterAdded)
                         CharacterAdded(LocalPlayer.Character)
                         
                         -- // Metatable \\ --
                         local RawMetatable = getrawmetatable(game)
                         local __Namecall = RawMetatable.__namecall
                         
                         setreadonly(RawMetatable, false)
                         
                         RawMetatable.__namecall = newcclosure(function(Object, ...)
                            local NamecallMethod = getnamecallmethod()
                            local Arguments = {...}
                         
                            if typeof(Object) == "Instance" and Object.IsA(Object, "RemoteEvent") then
                                if tostring(Object) == "Damage" and NamecallMethod == "FireServer" then
                                    Arguments[1].Damage = Arguments[1].BodyPart.Parent.Humanoid.MaxHealth + 10
                                end
                            end
                         
                            return __Namecall(Object, unpack(Arguments))
                         end)
                         
                         setreadonly(RawMetatable, true)
                         
                         -- // Actions \\ --
                         --LocalPlayer.Character.Health:Destroy()
                        end)






                    local page5 = venyx:addPage("Credits", 5012544693)
                    local Discord = page5:addSection("Discord")
                    
                    
                    Discord:addButton("Copy Discord Link", function()
                        setclipboard("https://discord.gg/JdEK5qN7Yb")
                    end)
                    local Discord = page5:addSection("Credits")
                    local d = page5:addSection("Made by: Leon!#0831")
                    local d = page5:addSection("Scripter: Leon!#0831")
                
                
                
                
                
                
                
                    -- themes
                    local themes = {
                        Background = Color3.fromRGB(24, 24, 24),
                        Glow = Color3.fromRGB(0, 0, 0),
                        Accent = Color3.fromRGB(10, 10, 10),
                        LightContrast = Color3.fromRGB(20, 20, 20),
                        DarkContrast = Color3.fromRGB(14, 14, 14),
                        TextColor = Color3.fromRGB(255, 255, 255)
                        }
                        
                            
                        -- Theme page
                        local settings = venyx:addPage("Settings", 5012544693)
                        local colors = settings:addSection("Colors")
                        local setting = settings:addSection("Settings")
                        
                        setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
                        print("Activated Keybind")
                        venyx:toggle()
                        end, function()
                        print("Changed Keybind")
                        end)
                        
                        for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
                        colors:addColorPicker(theme, color, function(color3)
                        venyx:setTheme(theme, color3)
                        end)
                        end
                        
                    -- load
                        venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
                        end























































                        if game.PlaceId == 192800 then
                            local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
                            local venyx = library.new("Vex Hub: Work At A Pizza Place", 5013109572)


    local section1 = venyx:addPage("Farming", 8162117119)
    local section1 = section1:addSection("Farming")



    section1:addButton("Auto Farm", function()
        if game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool") then
            for i, v in pairs(game.Players.LocalPlayer.Backpack:GetChildren()) do
                if v.Name:len() == 2 then
                    v.Parent = game.Players.LocalPlayer.Character
                end
            end
            
            wait()
            
            local item = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Tool").Name
            
                    for i,v in pairs(game.Workspace.Houses:GetChildren()) do
                        for i,v in pairs(v:GetChildren()) do
                            if v.Name == "Address" then
                                print(v.Value)
                                if v.Value == item then
                                    for i,v in pairs(v.Parent.Upgrades:GetChildren()) do
                                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = v.Parent.Parent.Upgrades[v.Name].Doors.FrontDoorMain.DoorTouch.CFrame * CFrame.new(-5, 0, 0)
                                        wait(8)
                                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(64.7216568, 6.63000107, -11.3157063)
                                        wait(1)
                                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(58.4695549, 6.63000107, -9.41764355)
                                        wait(1)
                                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(68.068222, 6.63000107, -7.46636343)
                                        wait(1)
                                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(68.9409561, 6.62999964, -11.2087212)
                                        wait(1)
                                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(64.0699768, 6.63000107, -11.5208979)
                                        wait(0.5)
                                        wait(1)
                                        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(59.1558762, 6.63000107, -7.64516211)
                                    end
                                end
                            end
                        end
                    end
        else
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(64.7216568, 6.63000107, -11.3157063)
            wait(1)
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(58.4695549, 6.63000107, -9.41764355)
            wait(1)
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(68.068222, 6.63000107, -7.46636343)
            wait(1)
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(68.9409561, 6.62999964, -11.2087212)
            wait(1)
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(64.0699768, 6.63000107, -11.5208979)
            wait(0.5)
            wait(1)
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(59.1558762, 6.63000107, -7.64516211)
            
        end
    

        end)




        
            
            



        section1:addButton("Delete all Players", function()
            loadstring(game:HttpGet("https://raw.githubusercontent.com/BlastingStone/MyLuaStuff/master/WaaPPEraser.lua"))()
            end)

            







                            local page5 = venyx:addPage("Credits", 5012544693)
                    local Discord = page5:addSection("Discord")
                    
                    
                    Discord:addButton("Copy Discord Link", function()
                        setclipboard("https://discord.gg/JdEK5qN7Yb")
                    end)
                    local Discord = page5:addSection("Credits")
                    local d = page5:addSection("Made by: Leon!#0831")
                    local d = page5:addSection("Scripter: Leon!#0831")
                
                
                
                
                
                
                
                    -- themes
                    local themes = {
                        Background = Color3.fromRGB(24, 24, 24),
                        Glow = Color3.fromRGB(0, 0, 0),
                        Accent = Color3.fromRGB(10, 10, 10),
                        LightContrast = Color3.fromRGB(20, 20, 20),
                        DarkContrast = Color3.fromRGB(14, 14, 14),
                        TextColor = Color3.fromRGB(255, 255, 255)
                        }
                        
                            
                        -- Theme page
                        local settings = venyx:addPage("Settings", 5012544693)
                        local colors = settings:addSection("Colors")
                        local setting = settings:addSection("Settings")
                        
                        setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
                        print("Activated Keybind")
                        venyx:toggle()
                        end, function()
                        print("Changed Keybind")
                        end)
                        
                        for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
                        colors:addColorPicker(theme, color, function(color3)
                        venyx:setTheme(theme, color3)
                        end)
                        end
                        
                    -- load
                        venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
                        end
        

































                        if game.PlaceId == 4520749081 then
                            local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
                            local venyx = library.new("Vex Hub: King Legacy", 5013109572)
                        
                            local section1 = venyx:addPage("Farming", 8162117119)
                            local section1 = section1:addSection("Farming")











                            local page5 = venyx:addPage("Credits", 5012544693)
                    local Discord = page5:addSection("Discord")
                    
                    
                    Discord:addButton("Copy Discord Link", function()
                        setclipboard("https://discord.gg/JdEK5qN7Yb")
                    end)
                    local Discord = page5:addSection("Credits")
                    local d = page5:addSection("Made by: Leon!#0831 and Zerio#0880")
                    local d = page5:addSection("Scripter: Zerio#0880")
                
                
                
                
                
                
                
                    -- themes
                    local themes = {
                        Background = Color3.fromRGB(24, 24, 24),
                        Glow = Color3.fromRGB(0, 0, 0),
                        Accent = Color3.fromRGB(10, 10, 10),
                        LightContrast = Color3.fromRGB(20, 20, 20),
                        DarkContrast = Color3.fromRGB(14, 14, 14),
                        TextColor = Color3.fromRGB(255, 255, 255)
                        }
                        
                            
                        -- Theme page
                        local settings = venyx:addPage("Settings", 5012544693)
                        local colors = settings:addSection("Colors")
                        local setting = settings:addSection("Settings")
                        
                        setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
                        print("Activated Keybind")
                        venyx:toggle()
                        end, function()
                        print("Changed Keybind")
                        end)
                        
                        for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
                        colors:addColorPicker(theme, color, function(color3)
                        venyx:setTheme(theme, color3)
                        end)
                        end
                        
                    -- load
                        venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
                        end




































                        if game.PlaceId == 2753915549 then
                            local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
                            local venyx = library.new("Vex Hub: Blox Fruits", 5013109572)
                        
                            local section1 = venyx:addPage("Farming", 8162117119)
                            local section1 = section1:addSection("Farming")















                            local page5 = venyx:addPage("Credits", 5012544693)
                    local Discord = page5:addSection("Discord")
                    
                    
                    Discord:addButton("Copy Discord Link", function()
                        setclipboard("https://discord.gg/JdEK5qN7Yb")
                    end)
                    local Discord = page5:addSection("Credits")
                    local d = page5:addSection("Made by: Leon!#0831 and Zerio#0880")
                    local d = page5:addSection("Scripter: Zerio#0880")
                
                
                
                
                
                
                
                    -- themes
                    local themes = {
                        Background = Color3.fromRGB(24, 24, 24),
                        Glow = Color3.fromRGB(0, 0, 0),
                        Accent = Color3.fromRGB(10, 10, 10),
                        LightContrast = Color3.fromRGB(20, 20, 20),
                        DarkContrast = Color3.fromRGB(14, 14, 14),
                        TextColor = Color3.fromRGB(255, 255, 255)
                        }
                        
                            
                        -- Theme page
                        local settings = venyx:addPage("Settings", 5012544693)
                        local colors = settings:addSection("Colors")
                        local setting = settings:addSection("Settings")
                        
                        setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
                        print("Activated Keybind")
                        venyx:toggle()
                        end, function()
                        print("Changed Keybind")
                        end)
                        
                        for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
                        colors:addColorPicker(theme, color, function(color3)
                        venyx:setTheme(theme, color3)
                        end)
                        end
                        
                    -- load
                        venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
                        end































                        if game.PlaceId == 221718525 then
                            local library = loadstring(game:HttpGet("https://raw.githubusercontent.com/zxciaz/VenyxUI/main/Reuploaded"))() --someone reuploaded it so I put it in place of the original back up so guy can get free credit.
                            local venyx = library.new("Vex Hub: Ninja Tycoon", 5013109572)
                        
                            local section1 = venyx:addPage("Farming", 817160385817119)
                            local section1 = section1:addSection("Farming")



                            section1:addButton("Auto Click on Convery", function()
                                function getTycoon()
                                    for i,v in pairs(game:GetService("Workspace")["Zednov's Tycoon Kit"].Tycoons:GetChildren()) do
                                        if v.Owner.Value == game:GetService("Players").LocalPlayer then
                                            return v
                                        end
                                    end
                                    return nil
                                end
                                 
                                local tycoon = getTycoon()
                                if tycoon == nil then
                                    repeat wait(0.5) tycoon = getTycoon() until tycoon ~= nil
                                end
                                 
                                repeat wait() until tycoon:FindFirstChild("PurchasedObjects") ~= nil
                                repeat wait() until tycoon.PurchasedObjects:FindFirstChild("Mine") ~= nil
                                 
                                while true do
                                    fireclickdetector(tycoon.PurchasedObjects.Mine.Clicker.ClickDetector)
                                    wait()
                                end
                            end)



                           






                            








                            local page5 = venyx:addPage("Credits", 5012544693)
                    local Discord = page5:addSection("Discord")
                    
                    
                    Discord:addButton("Copy Discord Link", function()
                        setclipboard("https://discord.gg/JdEK5qN7Yb")
                    end)
                    local Discord = page5:addSection("Credits")
                    local d = page5:addSection("Made by: Leon!#0831 and Zerio#0880")
                    local d = page5:addSection("Scripter: Zerio#0880")
                
                
                
                
                
                
                
                    -- themes
                    local themes = {
                        Background = Color3.fromRGB(24, 24, 24),
                        Glow = Color3.fromRGB(0, 0, 0),
                        Accent = Color3.fromRGB(10, 10, 10),
                        LightContrast = Color3.fromRGB(20, 20, 20),
                        DarkContrast = Color3.fromRGB(14, 14, 14),
                        TextColor = Color3.fromRGB(255, 255, 255)
                        }
                        
                            
                        -- Theme page
                        local settings = venyx:addPage("Settings", 8087082660)
                        local colors = settings:addSection("Colors")
                        local setting = settings:addSection("Settings")
                        
                        setting:addKeybind("Show/Hide Settings", Enum.KeyCode.P, function()
                        print("Activated Keybind")
                        venyx:toggle()
                        end, function()
                        print("Changed Keybind")
                        end)
                        
                        for theme, color in pairs(themes) do -- all in one theme changer, i know, im cool
                        colors:addColorPicker(theme, color, function(color3)
                        venyx:setTheme(theme, color3)
                        end)
                        end
                        
                    -- load
                        venyx:SelectPage(venyx.pages[1], true) -- no default for more freedom
                        end
