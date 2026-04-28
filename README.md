-- تحسين واجهة SH SPAM - نسخة سوداء شفافة مع زر الأرنب العائم
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local UIStroke = Instance.new("UIStroke")
local UICorner = Instance.new("UICorner")
local TextBox = Instance.new("TextBox")
local SpeedInput = Instance.new("TextBox")
local SpamButton = Instance.new("TextButton")
local SpeedLabel = Instance.new("TextLabel")
local Title = Instance.new("TextLabel")

-- إعدادات الحاوية الرئيسية
ScreenGui.Parent = game:GetService("CoreGui")
ScreenGui.Name = "SH_SPAM_UI"

MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(0, 0, 0) -- لون أسود
MainFrame.BackgroundTransparency = 0.25 -- درجة الشفافية (Glassmorphism)
MainFrame.Position = UDim2.new(0.5, -110, 0.5, -125)
MainFrame.Size = UDim2.new(0, 220, 0, 250)
MainFrame.Active = true
MainFrame.Draggable = true -- القائمة الرئيسية قابلة للسحب
MainFrame.BorderSizePixel = 0

UICorner.CornerRadius = UDim.new(0, 15)
UICorner.Parent = MainFrame

-- إطار مضيء خفيف لتمييز اللوحة الشفافة
UIStroke.Parent = MainFrame
UIStroke.Thickness = 1.5
UIStroke.Color = Color3.fromRGB(100, 100, 100)
UIStroke.Transparency = 0.5
UIStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

-- العنوان (تم تغيير الاسم إلى SH SPAM)
Title.Parent = MainFrame
Title.Size = UDim2.new(1, 0, 0, 45)
Title.Text = "SH SPAM"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.GothamBold
Title.TextSize = 20

-- مربع إدخال الرسالة
TextBox.Parent = MainFrame
TextBox.Position = UDim2.new(0.1, 0, 0.22, 0)
TextBox.Size = UDim2.new(0.8, 0, 0.18, 0)
TextBox.PlaceholderText = "اكتبي الرسالة هنا..."
TextBox.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
TextBox.BackgroundTransparency = 0.3
TextBox.TextColor3 = Color3.new(1, 1, 1)
TextBox.Font = Enum.Font.Gotham
TextBox.TextSize = 14
Instance.new("UICorner", TextBox).CornerRadius = UDim.new(0, 6)

-- عنوان السرعة
SpeedLabel.Parent = MainFrame
SpeedLabel.Position = UDim2.new(0.1, 0, 0.45, 0)
SpeedLabel.Size = UDim2.new(0.8, 0, 0.1, 0)
SpeedLabel.Text = "تأخير الإرسال (ثانية)"
SpeedLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
SpeedLabel.BackgroundTransparency = 1
SpeedLabel.Font = Enum.Font.Gotham
SpeedLabel.TextSize = 11

-- مربع إدخال الرقم
SpeedInput.Parent = MainFrame
SpeedInput.Position = UDim2.new(0.3, 0, 0.58, 0)
SpeedInput.Size = UDim2.new(0.4, 0, 0.12, 0)
SpeedInput.Text = "0.5"
SpeedInput.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
SpeedInput.BackgroundTransparency = 0.3
SpeedInput.TextColor3 = Color3.fromRGB(0, 255, 150)
SpeedInput.Font = Enum.Font.GothamBold
Instance.new("UICorner", SpeedInput).CornerRadius = UDim.new(0, 4)

-- زر التشغيل
SpamButton.Parent = MainFrame
SpamButton.Position = UDim2.new(0.1, 0, 0.78, 0)
SpamButton.Size = UDim2.new(0.8, 0, 0.16, 0)
SpamButton.Text = "بدء التشغيل"
SpamButton.Font = Enum.Font.GothamBold
SpamButton.TextColor3 = Color3.new(1, 1, 1)
SpamButton.TextSize = 15
Instance.new("UICorner", SpamButton).CornerRadius = UDim.new(0, 8)

-- تدرج لوني للزر ليبرز فوق الشفافية
local BtnGradient = Instance.new("UIGradient", SpamButton)
BtnGradient.Color = ColorSequence.new(Color3.fromRGB(120, 0, 255), Color3.fromRGB(70, 0, 180))

-----------------------------------------------------------
-- إنشاء زر الأرنب العائم (Mini Button) لالإخفاء/الإظهار
-----------------------------------------------------------
local ToggleButton = Instance.new("TextButton")
local ToggleCorner = Instance.new("UICorner")
local ToggleStroke = Instance.new("UIStroke")

ToggleButton.Name = "RabbitToggleButton"
ToggleButton.Parent = ScreenGui
-- مكان مبدئي للزر (أعلى اليسار مثلاً)
ToggleButton.Position = UDim2.new(0.1, 0, 0.1, 0)
ToggleButton.Size = UDim2.new(0, 50, 0, 50) -- زر دائري صغير
ToggleButton.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
ToggleButton.BackgroundTransparency = 0.2
ToggleButton.Text = "🐰" -- أيقونة الأرنب
ToggleButton.TextSize = 30
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.Active = true
ToggleButton.Draggable = true -- الزر العائم قابل للسحب

ToggleCorner.CornerRadius = UDim.new(1, 0) -- يجعله دائرياً تماماً
ToggleCorner.Parent = ToggleButton

ToggleStroke.Parent = ToggleButton
ToggleStroke.Thickness = 2
ToggleStroke.Color = Color3.fromRGB(120, 0, 255) -- لون الإطار بنفسجي مضيء
ToggleStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

-------------------------------- =المنطق البرمجي = --------------------------------

local spamming = false
-- التحقق الآمن من الريموتات
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local chatRemote = ReplicatedStorage:FindFirstChild("RemoteEvents") and ReplicatedStorage.RemoteEvents:FindFirstChild("ChatEvent")
local adminRemote = ReplicatedStorage:FindFirstChild("HDAdminHDClient") and ReplicatedStorage.HDAdminHDClient.Signals:FindFirstChild("RequestCommandModification")

-- وظيفة زر السبام (التشغيل/الإيقاف)
SpamButton.MouseButton1Click:Connect(function()
    spamming = not spamming
    if spamming then
        SpamButton.Text = "إيقاف"
        BtnGradient.Color = ColorSequence.new(Color3.fromRGB(255, 0, 0), Color3.fromRGB(150, 0, 0))
        UIStroke.Color = Color3.fromRGB(255, 0, 0) -- إطار أحمر عند التشغيل
        UIStroke.Transparency = 0.2
        
        task.spawn(function()
            while spamming do
                local text = TextBox.Text
                -- قراءة السرعة من مربع الإدخال
                local waitTime = tonumber(SpeedInput.Text) or 0.5
                if text ~= "" then
                    -- إرسال محاولاً عبر الريموتات المتاحة
                    if chatRemote then pcall(function() chatRemote:FireServer(text) end) end
                    if adminRemote then pcall(function() adminRemote:InvokeServer(text) end) end
                end
                task.wait(waitTime)
            end
        end)
    else
        SpamButton.Text = "بدء التشغيل"
        BtnGradient.Color = ColorSequence.new(Color3.fromRGB(120, 0, 255), Color3.fromRGB(70, 0, 180))
        UIStroke.Color = Color3.fromRGB(100, 100, 100) -- إعادة اللون الأصلي
        UIStroke.Transparency = 0.5
    end
end)

-- وظيفة زر الأرنب (الإخفاء والإظهار)
ToggleButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

-- تنظيف رسائل النظام (الحفاظ على الوظيفة السابقة)
game:GetService("CoreGui").DescendantAdded:Connect(function(obj)
    if obj:IsA("TextLabel") and (obj.Text:find("System") or obj.Text:find("limit")) then
        task.wait()
        pcall(function() obj.Parent:Destroy() end)
    end
end)
