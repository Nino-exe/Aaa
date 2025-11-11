-- X2ZU UI Framework (Corrigido para ImageButton no CloseUIButton)
-- Original: https://raw.githubusercontent.com/x2zu/OPEN-SOURCE-UI-ROBLOX/refs/heads/main/X2ZU%20UI%20ROBLOX%20OPEN%20SOURCE/DummyUi-leak-by-x2zu/fetching-main/Tools/Framework.luau
-- Modificação: Suporte a Image, Size e Transparency no CloseUIButton

local Library = {}

local TweenService = game:GetService("TweenService")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local CoreGui = game:GetService("CoreGui")

local WindowCount = 0

function Library:Window(options)
    WindowCount = WindowCount + 1
    
    local Window = {}
    local ScreenGui = Instance.new("ScreenGui")
    ScreenGui.Name = "X2ZU_UI_" .. WindowCount
    ScreenGui.ResetOnSpawn = false
    ScreenGui.DisplayOrder = 999999
    ScreenGui.Parent = CoreGui
    
    local MainFrame = Instance.new("Frame")
    MainFrame.Name = "MainFrame"
    MainFrame.Size = options.Config.Size or UDim2.new(0, 500, 0, 400)
    MainFrame.Position = UDim2.new(0.5, -MainFrame.Size.X.Offset/2, 0.5, -MainFrame.Size.Y.Offset/2)
    MainFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    MainFrame.BorderSizePixel = 0
    MainFrame.ClipsDescendants = true
    MainFrame.Active = true
    MainFrame.Draggable = true
    MainFrame.Parent = ScreenGui
    
    local UICorner = Instance.new("UICorner")
    UICorner.CornerRadius = UDim.new(0, 8)
    UICorner.Parent = MainFrame
    
    local TopBar = Instance.new("Frame")
    TopBar.Name = "TopBar"
    TopBar.Size = UDim2.new(1, 0, 0, 40)
    TopBar.Position = UDim2.new(0, 0, 0, 0)
    TopBar.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    TopBar.BorderSizePixel = 0
    TopBar.Parent = MainFrame
    
    local TopBarCorner = Instance.new("UICorner")
    TopBarCorner.CornerRadius = UDim.new(0, 8)
    TopBarCorner.Parent = TopBar
    
    local Title = Instance.new("TextLabel")
    Title.Name = "Title"
    Title.Size = UDim2.new(1, -80, 1, 0)
    Title.Position = UDim2.new(0, 10, 0, 0)
    Title.BackgroundTransparency = 1
    Title.Text = options.Title or "X2ZU UI"
    Title.TextColor3 = Color3.fromRGB(255, 255, 255)
    Title.TextXAlignment = Enum.TextXAlignment.Left
    Title.Font = Enum.Font.GothamBold
    Title.TextSize = 14
    Title.Parent = TopBar
    
    local Icon = Instance.new("ImageLabel")
    Icon.Name = "Icon"
    Icon.Size = UDim2.new(0, 24, 0, 24)
    Icon.Position = UDim2.new(0, 10, 0.5, -12)
    Icon.BackgroundTransparency = 1
    Icon.Image = "rbxassetid://" .. tostring(options.Icon or 4483345998)
    Icon.Parent = TopBar
    
    local Close = Instance.new("TextButton")
    Close.Name = "Close"
    Close.Size = UDim2.new(0, 30, 0, 30)
    Close.Position = UDim2.new(1, -40, 0, 5)
    Close.BackgroundColor3 = Color3.fromRGB(255, 85, 85)
    Close.Text = "×"
    Close.TextColor3 = Color3.fromRGB(255, 255, 255)
    Close.Font = Enum.Font.GothamBold
    Close.TextSize = 16
    Close.Parent = TopBar
    
    local CloseCorner = Instance.new("UICorner")
    CloseCorner.CornerRadius = UDim.new(0, 4)
    CloseCorner.Parent = Close
    
    local Container = Instance.new("Frame")
    Container.Name = "Container"
    Container.Size = UDim2.new(1, 0, 1, -40)
    Container.Position = UDim2.new(0, 0, 0, 40)
    Container.BackgroundTransparency = 1
    Container.Parent = MainFrame
    
    local TabContainer = Instance.new("Frame")
    TabContainer.Name = "TabContainer"
    TabContainer.Size = UDim2.new(0, 150, 1, 0)
    TabContainer.Position = UDim2.new(0, 0, 0, 0)
    TabContainer.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    TabContainer.BorderSizePixel = 0
    TabContainer.Parent = Container
    
    local TabContainerList = Instance.new("UIListLayout")
    TabContainerList.SortOrder = Enum.SortOrder.LayoutOrder
    TabContainerList.Padding = UDim.new(0, 0)
    TabContainerList.Parent = TabContainer
    
    local ContentContainer = Instance.new("Frame")
    ContentContainer.Name = "ContentContainer"
    ContentContainer.Size = UDim2.new(1, -150, 1, 0)
    ContentContainer.Position = UDim2.new(0, 150, 0, 0)
    ContentContainer.BackgroundTransparency = 1
    ContentContainer.Parent = Container
    
    local ContentContainerPadding = Instance.new("UIPadding")
    ContentContainerPadding.PaddingTop = UDim.new(0, 10)
    ContentContainerPadding.PaddingLeft = UDim.new(0, 10)
    ContentContainerPadding.PaddingRight = UDim.new(0, 10)
    ContentContainerPadding.PaddingBottom = UDim.new(0, 10)
    ContentContainerPadding.Parent = ContentContainer
    
    local ContentContainerGrid = Instance.new("UIGridLayout")
    ContentContainerGrid.CellSize = UDim2.new(1, -20, 0, 25)
    ContentContainerGrid.CellPadding = UDim2.new(0, 0, 0, 5)
    ContentContainerGrid.SortOrder = Enum.SortOrder.LayoutOrder
    ContentContainerGrid.Parent = ContentContainer
    
    local DescLabel = Instance.new("TextLabel")
    DescLabel.Name = "DescLabel"
    DescLabel.Size = UDim2.new(1, 0, 0, 20)
    DescLabel.Position = UDim2.new(0, 160, 0, 10)
    DescLabel.BackgroundTransparency = 1
    DescLabel.Text = options.Desc or ""
    DescLabel.TextColor3 = Color3.fromRGB(150, 150, 150)
    DescLabel.Font = Enum.Font.Gotham
    DescLabel.TextSize = 12
    DescLabel.TextXAlignment = Enum.TextXAlignment.Left
    DescLabel.Parent = Container
    
    local Tabs = {}
    local CurrentTab = nil
    
    function Window:Tab(options)
        local Tab = {}
        local TabButton = Instance.new("TextButton")
        TabButton.Name = options.Title .. "Tab"
        TabButton.Size = UDim2.new(1, 0, 0, 40)
        TabButton.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
        TabButton.Text = options.Icon and " " .. options.Icon .. " " .. options.Title or options.Title
        TabButton.TextColor3 = Color3.fromRGB(255, 255, 255)
        TabButton.Font = options.Icon and Enum.Font.Gotham or Enum.Font.GothamSemibold
        TabButton.TextSize = 13
        TabButton.TextXAlignment = Enum.TextXAlignment.Left
        TabButton.BorderSizePixel = 0
        TabButton.LayoutOrder = #Tabs + 1
        TabButton.Parent = TabContainer
        
        local TabButtonPadding = Instance.new("UIPadding")
        TabButtonPadding.PaddingLeft = UDim.new(0, 15)
        TabButtonPadding.Parent = TabButton
        
        local TabContent = Instance.new("Frame")
        TabContent.Name = options.Title .. "Content"
        TabContent.Size = UDim2.new(1, 0, 1, 0)
        TabContent.BackgroundTransparency = 1
        TabContent.Visible = false
        TabContent.Parent = ContentContainer
        
        local TabContentGrid = Instance.new("UIGridLayout")
        TabContentGrid.CellSize = UDim2.new(1, -20, 0, 25)
        TabContentGrid.CellPadding = UDim2.new(0, 0, 0, 5)
        TabContentGrid.SortOrder = Enum.SortOrder.LayoutOrder
        TabContentGrid.Parent = TabContent
        
        local TabContentPadding = Instance.new("UIPadding")
        TabContentPadding.PaddingTop = UDim.new(0, 10)
        TabContentPadding.PaddingLeft = UDim.new(0, 10)
        TabContentPadding.PaddingRight = UDim.new(0, 10)
        TabContentPadding.PaddingBottom = UDim.new(0, 10)
        TabContentPadding.Parent = TabContent
        
        Tab.Buttons = {}
        Tab.Content = TabContent
        Tab.Button = TabButton
        Tabs[#Tabs + 1] = Tab
        
        if #Tabs == 1 then
            CurrentTab = Tab
            TabContent.Visible = true
            TabButton.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
        end
        
        function Tab:Section(options)
            local Section = Instance.new("TextLabel")
            Section.Name = options.Title .. "Section"
            Section.Size = UDim2.new(1, 0, 0, 20)
            Section.BackgroundTransparency = 1
            Section.Text = options.Title
            Section.TextColor3 = Color3.fromRGB(200, 200, 200)
            Section.Font = Enum.Font.GothamBold
            Section.TextSize = 12
            Section.TextXAlignment = Enum.TextXAlignment.Left
            Section.LayoutOrder = #Tab.Buttons + 1
            Section.Parent = TabContent
            
            local SectionPadding = Instance.new("UIPadding")
            SectionPadding.PaddingLeft = UDim.new(0, 5)
            SectionPadding.Parent = Section
            
            Tab.Buttons[#Tab.Buttons + 1] = Section
        end
        
        function Tab:Toggle(options)
            local ToggleFrame = Instance.new("Frame")
            ToggleFrame.Name = options.Title .. "Toggle"
            ToggleFrame.Size = UDim2.new(1, 0, 0, 25)
            ToggleFrame.BackgroundTransparency = 1
            ToggleFrame.LayoutOrder = #Tab.Buttons + 1
            ToggleFrame.Parent = TabContent
            
            local ToggleButton = Instance.new("TextButton")
            ToggleButton.Name = "ToggleButton"
            ToggleButton.Size = UDim2.new(0, 20, 0, 20)
            ToggleButton.Position = UDim2.new(1, -25, 0.5, -10)
            ToggleButton.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
            ToggleButton.Text = ""
            ToggleButton.Font = Enum.Font.Gotham
            ToggleButton.TextSize = 12
            ToggleButton.BorderSizePixel = 0
            ToggleButton.Parent = ToggleFrame
            
            local ToggleCorner = Instance.new("UICorner")
            ToggleCorner.CornerRadius = UDim.new(1, 0)
            ToggleCorner.Parent = ToggleButton
            
            local ToggleLabel = Instance.new("TextLabel")
            ToggleLabel.Name = "ToggleLabel"
            ToggleLabel.Size = UDim2.new(1, -50, 1, 0)
            ToggleLabel.BackgroundTransparency = 1
            ToggleLabel.Text = options.Title
            ToggleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
            ToggleLabel.Font = Enum.Font.Gotham
            ToggleLabel.TextSize = 12
            ToggleLabel.TextXAlignment = Enum.TextXAlignment.Left
            ToggleLabel.Parent = ToggleFrame
            
            local ToggleDesc = Instance.new("TextLabel")
            ToggleDesc.Name = "ToggleDesc"
            ToggleDesc.Size = UDim2.new(1, -50, 0, 15)
            ToggleDesc.Position = UDim2.new(0, 0, 1, 0)
            ToggleDesc.BackgroundTransparency = 1
            ToggleDesc.Text = options.Desc or ""
            ToggleDesc.TextColor3 = Color3.fromRGB(150, 150, 150)
            ToggleDesc.Font = Enum.Font.Gotham
            ToggleDesc.TextSize = 10
            ToggleDesc.TextXAlignment = Enum.TextXAlignment.Left
            ToggleDesc.Parent = ToggleFrame
            
            local ToggleIndicator = Instance.new("Frame")
            ToggleIndicator.Name = "Indicator"
            ToggleIndicator.Size = UDim2.new(0, 16, 0, 16)
            ToggleIndicator.Position = UDim2.new(0, 2, 0, 2)
            ToggleIndicator.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
            ToggleIndicator.BorderSizePixel = 0
            ToggleIndicator.Visible = false
            ToggleIndicator.Parent = ToggleButton
            
            local IndicatorCorner = Instance.new("UICorner")
            IndicatorCorner.CornerRadius = UDim.new(1, 0)
            IndicatorCorner.Parent = ToggleIndicator
            
            local value = options.Value or false
            local callback = options.Callback or function() end
            
            local function updateToggle()
                value = not value
                ToggleIndicator.Visible = value
                if value then
                    TweenService:Create(ToggleButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(0, 255, 0)}):Play()
                else
                    TweenService:Create(ToggleButton, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(150, 150, 150)}):Play()
                end
                callback(value)
            end
            
            ToggleButton.MouseButton1Click:Connect(updateToggle)
            
            Tab.Buttons[#Tab.Buttons + 1] = ToggleFrame
            
            return setmetatable({}, {
                __index = function() end,
                __newindex = function() end
            })
        end
        
        function Tab:Button(options)
            local Button = Instance.new("TextButton")
            Button.Name = options.Title .. "Button"
            Button.Size = UDim2.new(1, 0, 0, 30)
            Button.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
            Button.Text = options.Title
            Button.TextColor3 = Color3.fromRGB(255, 255, 255)
            Button.Font = Enum.Font.Gotham
            Button.TextSize = 12
            Button.BorderSizePixel = 0
            Button.LayoutOrder = #Tab.Buttons + 1
            Button.Parent = TabContent
            
            local ButtonCorner = Instance.new("UICorner")
            ButtonCorner.CornerRadius = UDim.new(0, 4)
            ButtonCorner.Parent = Button
            
            local ButtonDesc = Instance.new("TextLabel")
            ButtonDesc.Name = "ButtonDesc"
            ButtonDesc.Size = UDim2.new(1, 0, 0, 15)
            ButtonDesc.Position = UDim2.new(0, 0, 1, 0)
            ButtonDesc.BackgroundTransparency = 1
            ButtonDesc.Text = options.Desc or ""
            ButtonDesc.TextColor3 = Color3.fromRGB(150, 150, 150)
            ButtonDesc.Font = Enum.Font.Gotham
            ButtonDesc.TextSize = 10
            ButtonDesc.TextXAlignment = Enum.TextXAlignment.Left
            ButtonDesc.Parent = Button
            
            local ButtonPadding = Instance.new("UIPadding")
            ButtonPadding.PaddingLeft = UDim.new(0, 10)
            ButtonPadding.PaddingRight = UDim.new(0, 10)
            ButtonPadding.PaddingTop = UDim.new(0, 8)
            ButtonPadding.PaddingBottom = UDim.new(0, 8)
            ButtonPadding.Parent = Button
            
            local callback = options.Callback or function() end
            
            Button.MouseButton1Click:Connect(function()
                TweenService:Create(Button, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(55, 55, 55)}):Play()
                wait(0.1)
                TweenService:Create(Button, TweenInfo.new(0.1), {BackgroundColor3 = Color3.fromRGB(45, 45, 45)}):Play()
                callback()
            end)
            
            Tab.Buttons[#Tab.Buttons + 1] = Button
            
            return setmetatable({}, {
                __index = function() end,
                __newindex = function() end
            })
        end
        
        function Tab:Slider(options)
            local SliderFrame = Instance.new("Frame")
            SliderFrame.Name = options.Title .. "Slider"
            SliderFrame.Size = UDim2.new(1, 0, 0, 40)
            SliderFrame.BackgroundTransparency = 1
            SliderFrame.LayoutOrder = #Tab.Buttons + 1
            SliderFrame.Parent = TabContent
            
            local SliderLabel = Instance.new("TextLabel")
            SliderLabel.Name = "SliderLabel"
            SliderLabel.Size = UDim2.new(1, 0, 0, 15)
            SliderLabel.BackgroundTransparency = 1
            SliderLabel.Text = options.Title
            SliderLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
            SliderLabel.Font = Enum.Font.Gotham
            SliderLabel.TextSize = 12
            SliderLabel.TextXAlignment = Enum.TextXAlignment.Left
            SliderLabel.Parent = SliderFrame
            
            local SliderBar = Instance.new("Frame")
            SliderBar.Name = "SliderBar"
            SliderBar.Size = UDim2.new(1, 0, 0, 6)
            SliderBar.Position = UDim2.new(0, 0, 0.5, 5)
            SliderBar.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
            SliderBar.BorderSizePixel = 0
            SliderBar.Parent = SliderFrame
            
            local SliderBarCorner = Instance.new("UICorner")
            SliderBarCorner.CornerRadius = UDim.new(0, 3)
            SliderBarCorner.Parent = SliderBar
            
            local SliderFill = Instance.new("Frame")
            SliderFill.Name = "SliderFill"
            SliderFill.Size = UDim2.new((options.Default - options.Min) / (options.Max - options.Min), 0, 1, 0)
            SliderFill.Position = UDim2.new(0, 0, 0, 0)
            SliderFill.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
            SliderFill.BorderSizePixel = 0
            SliderFill.Parent = SliderBar
            
            local SliderFillCorner = Instance.new("UICorner")
            SliderFillCorner.CornerRadius = UDim.new(0, 3)
            SliderFillCorner.Parent = SliderFill
            
            local SliderValue = Instance.new("TextLabel")
            SliderValue.Name = "SliderValue"
            SliderValue.Size = UDim2.new(0, 50, 0, 15)
            SliderValue.Position = UDim2.new(1, -55, 0.5, 5)
            SliderValue.BackgroundTransparency = 1
            SliderValue.Text = tostring(options.Default)
            SliderValue.TextColor3 = Color3.fromRGB(255, 255, 255)
            SliderValue.Font = Enum.Font.Gotham
            SliderValue.TextSize = 11
            SliderValue.Parent = SliderFrame
            
            local SliderDesc = Instance.new("TextLabel")
            SliderDesc.Name = "SliderDesc"
            SliderDesc.Size = UDim2.new(1, 0, 0, 12)
            SliderDesc.Position = UDim2.new(0, 0, 1, 0)
            SliderDesc.BackgroundTransparency = 1
            SliderDesc.Text = options.Desc or ""
            SliderDesc.TextColor3 = Color3.fromRGB(150, 150, 150)
            SliderDesc.Font = Enum.Font.Gotham
            SliderDesc.TextSize = 10
            SliderDesc.TextXAlignment = Enum.TextXAlignment.Left
            SliderDesc.Parent = SliderFrame
            
            local dragging = false
            local value = options.Default or options.Min
            local callback = options.Callback or function() end
            
            local function updateSlider(input)
                local relativeX = math.clamp((input.Position.X - SliderBar.AbsolutePosition.X) / SliderBar.AbsoluteSize.X, 0, 1)
                value = math.floor(options.Min + (options.Max - options.Min) * relativeX)
                SliderFill.Size = UDim2.new(relativeX, 0, 1, 0)
                SliderValue.Text = tostring(value)
                callback(value)
            end
            
            SliderBar.InputBegan:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    dragging = true
                    updateSlider(input)
                end
            end)
            
            UserInputService.InputChanged:Connect(function(input)
                if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
                    updateSlider(input)
                end
            end)
            
            UserInputService.InputEnded:Connect(function(input)
                if input.UserInputType == Enum.UserInputType.MouseButton1 then
                    dragging = false
                end
            end)
            
            Tab.Buttons[#Tab.Buttons + 1] = SliderFrame
            
            return setmetatable({}, {
                __index = function() end,
                __newindex = function() end
            })
        end
        
        TabButton.MouseButton1Click:Connect(function()
            if CurrentTab ~= Tab then
                CurrentTab.Content.Visible = false
                CurrentTab.Button.BackgroundColor3 = Color3.fromRGB(45, 45, 45)
                TabContent.Visible = true
                TabButton.BackgroundColor3 = Color3.fromRGB(55, 55, 55)
                CurrentTab = Tab
            end
        end)
        
        return Tab
    end
    
    function Window:Notify(options)
        local Notification = Instance.new("Frame")
        Notification.Name = "Notification"
        Notification.Size = UDim2.new(0, 250, 0, 80)
        Notification.Position = UDim2.new(1, -270, 0, 20)
        Notification.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        Notification.Borde
