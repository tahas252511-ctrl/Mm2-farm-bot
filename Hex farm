-- ============================================
-- 🎯 AKILLI FARM + MURDER SCRIPT
-- Özellik: 50 coin topla → Sonra katilken herkesi kes
-- GUI: RAYFIELD (Modern ve Türkçe)
-- Yazar: AI Assistant
-- ============================================

getgenv().SecureMode = true

-- Rayfield Library yükle
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Ana Pencere oluştur
local Window = Rayfield:CreateWindow({
    Name = "🤖 Akıllı FarmBot",
    LoadingTitle = "Akıllı Farm Sistemi Yükleniyor...",
    LoadingSubtitle = "50 Coin → Tüm Katil",
    ConfigurationSaving = { 
        Enabled = false,
        FolderName = "AkilliFarmBot",
        FileName = "Ayarlar"
    },
    Discord = {
        Enabled = false,
        Invite = "noinvitelink",
        RememberJoins = true
    },
    KeySystem = false,
    KeySettings = {
        Title = "Anahtar Sistemi",
        Subtitle = "Anahtar girin",
        Note = "Anahtar gerekli değil",
        FileName = "Key",
        SaveKey = false,
        GrabKeyFromSite = false,
        Key = "1234"
    }
})

-- Değişkenler
local Player = game:GetService("Players").LocalPlayer
local Workspace = game:GetService("Workspace")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

-- Sistem Değişkenleri
local AutoFarm = false
local TargetCoins = 50
local CollectedCoins = 0
local IsMurderer = false
local MurderMode = false
local KillList = {}
local FailedKills = {}
local TotalKills = 0
local StartTime = 0

-- Sekmeler oluştur
local MainTab = Window:CreateTab("🤖 Ana Kontrol", 4483362458)
local SettingsTab = Window:CreateTab("⚙️ Ayarlar", 4483362458)
local StatsTab = Window:CreateTab("📊 İstatistikler", 4483362458)
local VisualTab = Window:CreateTab("🎨 Görsel", 4483362458)

-- ============================
-- 🤖 ANA KONTROL SİSTEMİ
-- ============================

MainTab:CreateSection("🎮 Kontrol Paneli")

-- Ana Toggle
local MainToggle = MainTab:CreateToggle({
    Name = "🚀 Akıllı FarmBot'u Başlat",
    CurrentValue = false,
    Flag = "MainToggle",
    Callback = function(Value)
        AutoFarm = Value
        if Value then
            StartTime = tick()
            StartSmartSystem()
            Rayfield:Notify({
                Title = "✅ Sistem Başlatıldı",
                Content = "50 coin toplanacak, sonra katil modu aktif olacak!",
                Duration = 5,
                Image = 4483362458
            })
        else
            Rayfield:Notify({
                Title = "⛔ Sistem Durduruldu",
                Content = "Tüm otomasyon durduruldu!",
                Duration = 3,
                Image = 4483362458
            })
        end
    end,
})

-- Hedef Coin Ayarlama
local TargetCoinSlider = MainTab:CreateSlider({
    Name = "🎯 Hedef Coin Sayısı",
    Range = {10, 200},
    Increment = 5,
    Suffix = "Coin",
    CurrentValue = 50,
    Flag = "TargetCoins",
    Callback = function(Value)
        TargetCoins = Value
        ProgressLabel:Set("📊 İlerleme: " .. CollectedCoins .. "/" .. TargetCoins .. " coin")
    end,
})

-- Durum Göstergesi
local StatusLabel = MainTab:CreateLabel("🔴 Sistem: DURDU")
local ProgressLabel = MainTab:CreateLabel("📊 İlerleme: 0/" .. TargetCoins .. " coin")

MainTab:CreateSection("⚡ Hızlı Ayarlar")

-- Hızlı Başlat Butonları
MainTab:CreateButton({
    Name = "▶️ Hemen Başlat (Varsayılan)",
    Callback = function()
        MainToggle:Set(true)
        Rayfield:Notify({
            Title = "⚡ Hızlı Başlat",
            Content = "Varsayılan ayarlarla başlatılıyor...",
            Duration = 3,
            Image = 4483362458
        })
    end,
})

MainTab:CreateButton({
    Name = "⏸️ Sistemi Durdur",
    Callback = function()
        MainToggle:Set(false)
    end,
})

MainTab:CreateButton({
    Name = "🔄 Sıfırla ve Yeniden Başlat",
    Callback = function()
        CollectedCoins = 0
        TotalKills = 0
        KillList = {}
        FailedKills = {}
        MurderMode = false
        
        CoinCountLabel:Set("💰 Toplanan Coin: 0")
        KillCountLabel:Set("🔪 Öldürülen Kişi: 0")
        FailedKillsLabel:Set("❌ Başarısız Deneme: 0")
        ProgressLabel:Set("📊 İlerleme: 0/" .. TargetCoins .. " coin")
        StatusLabel:Set("🔄 SİSTEM SIFIRLANDI")
        
        Rayfield:Notify({
            Title = "🔄 Sistem Sıfırlandı",
            Content = "Tüm istatistikler sıfırlandı!",
            Duration = 3,
            Image = 4483362458
        })
    end,
})

-- ============================
-- ⚙️ AYARLAR SİSTEMİ
-- ============================

SettingsTab:CreateSection("🌾 Farm Ayarları")

-- Farm Hızı
local FarmSpeedSlider = SettingsTab:CreateSlider({
    Name = "⚡ Farm Hızı",
    Range = {0.1, 1.0},
    Increment = 0.1,
    Suffix = "saniye",
    CurrentValue = 0.3,
    Flag = "FarmSpeed",
    Callback = function(Value)
        getgenv().FarmSpeed = Value
    end,
})

-- Farm Mesafesi
local FarmRangeSlider = SettingsTab:CreateSlider({
    Name = "📏 Farm Mesafesi",
    Range = {20, 300},
    Increment = 10,
    Suffix = "stud",
    CurrentValue = 100,
    Flag = "FarmRange",
    Callback = function(Value)
        getgenv().FarmRange = Value
    end,
})

SettingsTab:CreateSection("🔪 Katil Ayarları")

-- Öldürme Mesafesi
local KillRangeSlider = SettingsTab:CreateSlider({
    Name = "🔪 Öldürme Mesafesi",
    Range = {5, 50},
    Increment = 1,
    Suffix = "stud",
    CurrentValue = 15,
    Flag = "KillRange",
    Callback = function(Value)
        getgenv().KillRange = Value
    end,
})

-- Bekleme Süresi
local KillCooldownSlider = SettingsTab:CreateSlider({
    Name = "⏱️ Saldırı Beklemesi",
    Range = {0.5, 3},
    Increment = 0.1,
    Suffix = "saniye",
    CurrentValue = 1,
    Flag = "KillCooldown",
    Callback = function(Value)
        getgenv().KillCooldown = Value
    end,
})

SettingsTab:CreateSection("🔄 Hata Yönetimi")

-- Hata Yönetimi
local RetryToggle = SettingsTab:CreateToggle({
    Name = "🔄 Başarısızları Tekrar Dene",
    CurrentValue = true,
    Flag = "RetryToggle",
    Callback = function(Value)
        getgenv().RetryFailed = Value
    end,
})

local MaxRetriesSlider = SettingsTab:CreateSlider({
    Name = "🔄 Maksimum Deneme",
    Range = {1, 10},
    Increment = 1,
    Suffix = "kez",
    CurrentValue = 3,
    Flag = "MaxRetries",
    Callback = function(Value)
        getgenv().MaxRetries = Value
    end,
})

-- ============================
-- 📊 İSTATİSTİKLER SİSTEMİ
-- ============================

StatsTab:CreateSection("📈 Canlı İstatistikler")

local CoinCountLabel = StatsTab:CreateLabel("💰 Toplanan Coin: 0")
local KillCountLabel = StatsTab:CreateLabel("🔪 Öldürülen Kişi: 0")
local FailedKillsLabel = StatsTab:CreateLabel("❌ Başarısız Deneme: 0")
local ModeLabel = StatsTab:CreateLabel("🎯 Mod: FARM MODU")
local EfficiencyLabel = StatsTab:CreateLabel("📈 Verimlilik: 0%")

-- Sistem Durumu
StatsTab:CreateSection("⚙️ Sistem Durumu")
local SystemStatusLabel = StatsTab:CreateLabel("🟢 Sistem: HAZIR")
local TimeLabel = StatsTab:CreateLabel("⏰ Çalışma Süresi: 0s")
local PlayersLeftLabel = StatsTab:CreateLabel("👥 Kalan Oyuncu: 0")

-- ============================
-- 🎨 GÖRSEL AYARLAR
-- ============================

VisualTab:CreateSection("🎭 Görsel Efektler")

local VisualEffectsToggle = VisualTab:CreateToggle({
    Name = "✨ Görsel Efektler",
    CurrentValue = true,
    Flag = "VisualEffects",
    Callback = function(Value)
        getgenv().VisualEffects = Value
    end,
})

local NotificationsToggle = VisualTab:CreateToggle({
    Name = "🔔 Bildirimler",
    CurrentValue = true,
    Flag = "Notifications",
    Callback = function(Value)
        getgenv().Notifications = Value
    end,
})

VisualTab:CreateSection("🎨 Tema")

local ThemeDropdown = VisualTab:CreateDropdown({
    Name = "🎨 Tema Rengi",
    Options = {"Koyu", "Açık", "Mavi", "Kırmızı", "Yeşil"},
    CurrentOption = "Koyu",
    Flag = "ThemeColor",
    Callback = function(Option)
        -- Tema değiştirme mantığı
        Rayfield:Notify({
            Title = "🎨 Tema Değiştirildi",
            Content = "Yeni tema: " .. Option,
            Duration = 2,
            Image = 4483362458
        })
    end,
})

-- ============================
-- 🧠 AKILLI SİSTEM FONKSİYONLARI
-- ============================

-- Katil Kontrolü
local function CheckIfMurderer()
    local char = Player.Character
    if not char then return false end
    
    -- Bıçak kontrolü
    for _, child in pairs(char:GetChildren()) do
        if child.Name == "Knife" or child.Name:find("Knife") or child.Name:find("Sword") then
            return true
        end
    end
    
    -- Sırt çantası kontrolü
    local backpack = char:FindFirstChild("Backpack")
    if backpack then
        for _, tool in pairs(backpack:GetChildren()) do
            if tool.Name == "Knife" then
                return true
            end
        end
    end
    
    return false
end

-- Tüm Oyuncuları Listele
local function GetAllPlayers()
    local players = {}
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= Player and player.Character then
            local char = player.Character
            if char:FindFirstChild("Humanoid") and char.Humanoid.Health > 0 then
                if not CheckIfMurdererForPlayer(player) then  -- Sadece masumlar
                    table.insert(players, player)
                end
            end
        end
    end
    return players
end

-- Oyuncunun katil olup olmadığını kontrol et
local function CheckIfMurdererForPlayer(targetPlayer)
    if not targetPlayer or not targetPlayer.Character then return false end
    
    local char = targetPlayer.Character
    for _, child in pairs(char:GetChildren()) do
        if child.Name == "Knife" then
            return true
        end
    end
    return false
end

-- Coin Toplama Fonksiyonu
local function CollectCoins()
    local coinsCollectedThisCycle = 0
    
    -- Tüm coin'leri bul
    for _, obj in pairs(Workspace:GetDescendants()) do
        if not AutoFarm then break end
        
        if obj:IsA("BasePart") and (obj.Name == "Coin" or obj.Name == "GoldCoin") then
            local char = Player.Character
            if char and char:FindFirstChild("HumanoidRootPart") then
                local distance = (char.HumanoidRootPart.Position - obj.Position).Magnitude
                
                if distance <= (getgenv().FarmRange or 100) then
                    -- Coin'i topla
                    firetouchinterest(char.HumanoidRootPart, obj, 0)
                    firetouchinterest(char.HumanoidRootPart, obj, 1)
                    
                    coinsCollectedThisCycle = coinsCollectedThisCycle + 1
                    CollectedCoins = CollectedCoins + 1
                    
                    -- Görsel efekt
                    if getgenv().VisualEffects ~= false then
                        local effect = Instance.new("Part")
                        effect.Size = Vector3.new(1, 1, 1)
                        effect.Position = obj.Position
                        effect.Anchored = true
                        effect.CanCollide = false
                        effect.Material = Enum.Material.Neon
                        effect.Color = obj.Name == "GoldCoin" and Color3.fromRGB(255, 215, 0) or Color3.fromRGB(255, 255, 0)
                        effect.Parent = Workspace
                        
                        game:GetService("Debris"):AddItem(effect, 1)
                    end
                    
                    -- İstatistikleri güncelle
                    CoinCountLabel:Set("💰 Toplanan Coin: " .. CollectedCoins)
                    ProgressLabel:Set("📊 İlerleme: " .. CollectedCoins .. "/" .. TargetCoins .. " coin")
                    
                    -- Verimlilik hesapla
                    local efficiency = math.floor((CollectedCoins / TargetCoins) * 100)
                    EfficiencyLabel:Set("📈 Verimlilik: " .. efficiency .. "%")
                    
                    task.wait(getgenv().FarmSpeed or 0.3)
                    
                    -- Hedefe ulaştı mı kontrol et
                    if CollectedCoins >= TargetCoins then
                        StatusLabel:Set("🎯 HEDEFE ULAŞILDI! Katil modu bekleniyor...")
                        ModeLabel:Set("🎯 Mod: KATİL MODU BEKLENİYOR")
                        MurderMode = true
                        
                        if getgenv().Notifications ~= false then
                            Rayfield:Notify({
                                Title = "🎯 HEDEFE ULAŞILDI!",
                                Content = TargetCoins .. " coin toplandı! Katil olunca herkes öldürülecek!",
                                Duration = 5,
                                Image = 4483362458
                            })
                        end
                        
                        return true
                    end
                end
            end
        end
    end
    
    return coinsCollectedThisCycle > 0
end

-- Oyuncu Öldürme Fonksiyonu
local function KillPlayer(targetPlayer)
    local success = false
    
    if not targetPlayer or not targetPlayer.Character then
        return false
    end
    
    local char = Player.Character
    local targetChar = targetPlayer.Character
    
    if not char or not char:FindFirstChild("HumanoidRootPart") then
        return false
    end
    
    if not targetChar:FindFirstChild("HumanoidRootPart") then
        return false
    end
    
    -- Mesafe kontrolü
    local distance = (char.HumanoidRootPart.Position - targetChar.HumanoidRootPart.Position).Magnitude
    
    if distance > (getgenv().KillRange or 15) then
        -- Hedefe yaklaş
        char.HumanoidRootPart.CFrame = targetChar.HumanoidRootPart.CFrame * CFrame.new(0, 0, -2)
        task.wait(0.2)
    end
    
    -- Öldürme denemesi
    for attempt = 1, (getgenv().MaxRetries or 3) do
        if not AutoFarm then break end
        
        -- Görsel feedback
        if getgenv().VisualEffects ~= false then
            local highlight = Instance.new("Highlight")
            highlight.FillColor = Color3.fromRGB(255, 0, 0)
            highlight.OutlineColor = Color3.fromRGB(255, 255, 255)
            highlight.FillTransparency = 0.5
            highlight.Parent = targetChar
            game:GetService("Debris"):AddItem(highlight, 1)
        end
        
        -- Remote event'leri dene
        local remotes = {
            "KnifeRemote",
            "DamagePlayer",
            "HitPlayer",
            "Damage",
            "Hit",
            "MurderHit"
        }
        
        for _, remoteName in pairs(remotes) do
            local remote = game:GetService("ReplicatedStorage"):FindFirstChild(remoteName)
            if not remote then
                remote = game:GetService("Workspace"):FindFirstChild(remoteName)
            end
            
            if remote and remote:IsA("RemoteEvent") then
                pcall(function()
                    remote:FireServer(targetChar.HumanoidRootPart)
                    success = true
                end)
            end
        end
        
        -- Mouse click simülasyonu
        if char:FindFirstChild("Knife") then
            local knife = char.Knife
            pcall(function()
                knife:Activate()
                success = true
            end)
        end
        
        -- Touch simulation
        if not success then
            pcall(function()
                firetouchinterest(char.HumanoidRootPart, targetChar.HumanoidRootPart, 0)
                firetouchinterest(char.HumanoidRootPart, targetChar.HumanoidRootPart, 1)
                success = true
            end)
        end
        
        if success then
            -- Başarılı öldürme
            KillList[targetPlayer.Name] = true
            FailedKills[targetPlayer.Name] = nil
            TotalKills = TotalKills + 1
            
            -- İstatistik güncelle
            KillCountLabel:Set("🔪 Öldürülen Kişi: " .. TotalKills)
            
            if getgenv().Notifications ~= false then
                Rayfield:Notify({
                    Title = "✅ ÖLDÜRÜLDÜ",
                    Content = targetPlayer.Name .. " öldürüldü! (" .. attempt .. ". deneme)",
                    Duration = 2,
                    Image = 4483362458
                })
            end
            
            return true
        else
            -- Başarısız deneme
            FailedKills[targetPlayer.Name] = (FailedKills[targetPlayer.Name] or 0) + 1
            FailedKillsLabel:Set("❌ Başarısız Deneme: " .. table.size(FailedKills))
            
            if getgenv().Notifications ~= false then
                Rayfield:Notify({
                    Title = "❌ BAŞARISIZ",
                    Content = targetPlayer.Name .. " öldürülemedi (" .. attempt .. ". deneme)",
                    Duration = 1,
                    Image = 4483362458
                })
            end
            
            task.wait(getgenv().KillCooldown or 1)
        end
    end
    
    return false
end

-- Tüm Oyuncuları Öldür
local function KillAllPlayers()
    if not CheckIfMurderer() then
        StatusLabel:Set("⏳ KATİL DEĞİLSİN! Bekleniyor...")
        ModeLabel:Set("🎯 Mod: KATİL BEKLENİYOR")
        return false
    end
    
    StatusLabel:Set("🔪 KATİL MODU AKTİF! Herkes öldürülüyor...")
    ModeLabel:Set("🎯 Mod: KATİL MODU AKTİF")
    
    local allPlayers = GetAllPlayers()
    local killedThisCycle = 0
    
    PlayersLeftLabel:Set("👥 Kalan Oyuncu: " .. #allPlayers)
    
    -- Önce başarısız olanları dene
    if getgenv().RetryFailed ~= false then
        for playerName, retryCount in pairs(FailedKills) do
            if retryCount < (getgenv().MaxRetries or 3) then
                local player = game.Players:FindFirstChild(playerName)
                if player and not KillList[player.Name] then
                    if KillPlayer(player) then
                        killedThisCycle = killedThisCycle + 1
                    end
                    task.wait(getgenv().KillCooldown or 1)
                end
            end
        end
    end
    
    -- Sonra diğer oyuncuları öldür
    for _, player in pairs(allPlayers) do
        if not AutoFarm then break end
        
        if not KillList[player.Name] then
            if KillPlayer(player) then
                killedThisCycle = killedThisCycle + 1
            end
            
            task.wait(getgenv().KillCooldown or 1)
        end
    end
    
    -- Tüm oyuncular öldürüldü mü kontrol et
    local remainingPlayers = #GetAllPlayers()
    PlayersLeftLabel:Set("👥 Kalan Oyuncu: " .. remainingPlayers)
    
    if remainingPlayers == 0 then
        StatusLabel:Set("🎉 TÜM OYUNCULAR ÖLDÜRÜLDÜ!")
        
        if getgenv().Notifications ~= false then
            Rayfield:Notify({
                Title = "🎉 MİSYON TAMAMLANDI!",
                Content = "Tüm oyuncular öldürüldü! Sistem yeniden başlatılıyor...",
                Duration = 5,
                Image = 4483362458
            })
        end
        
        -- Sistem yeniden başlat
        task.wait(3)
        CollectedCoins = 0
        KillList = {}
        FailedKills = {}
        MurderMode = false
        
        CoinCountLabel:Set("💰 Toplanan Coin: 0")
        KillCountLabel:Set("🔪 Öldürülen Kişi: " .. TotalKills)
        FailedKillsLabel:Set("❌ Başarısız Deneme: 0")
        ProgressLabel:Set("📊 İlerleme: 0/" .. TargetCoins .. " coin")
        StatusLabel:Set("🔄 SİSTEM YENİDEN BAŞLATILIYOR...")
        EfficiencyLabel:Set("📈 Verimlilik: 0%")
        
        return true
    end
    
    return false
end

-- ============================
-- 🚀 ANA SİSTEM DÖNGÜSÜ
-- ============================

local function StartSmartSystem()
    coroutine.wrap(function()
        StartTime = tick()
        
        while AutoFarm do
            -- Çalışma süresini güncelle
            local elapsedTime = math.floor(tick() - StartTime)
            local minutes = math.floor(elapsedTime / 60)
            local seconds = elapsedTime % 60
            TimeLabel:Set("⏰ Çalışma Süresi: " .. minutes .. "d " .. seconds .. "s")
            
            if MurderMode then
                -- KATİL MODU
                SystemStatusLabel:Set("🔴 Sistem: KATİL MODU")
                
                if CheckIfMurderer() then
                    -- Katilse herkesi öldür
                    local allKilled = KillAllPlayers()
                    
                    if allKilled then
                        -- Tüm oyuncular öldü, farm moduna dön
                        MurderMode = false
                        StatusLabel:Set("🔄 Farm Moduna Dönülüyor...")
                    end
                else
                    -- Katil değilse bekle
                    StatusLabel:Set("⏳ KATİL DEĞİLSİN! Bekleniyor...")
                    task.wait(2)
                end
            else
                -- FARM MODU
                SystemStatusLabel:Set("🟢 Sistem: FARM MODU")
                
                if CollectedCoins < TargetCoins then
                    StatusLabel:Set("💰 COİN TOPLANIYOR: " .. CollectedCoins .. "/" .. TargetCoins)
                    
                    -- Coin topla
                    local foundCoins = CollectCoins()
                    
                    if not foundCoins then
                        -- Coin yoksa, biraz bekle
                        StatusLabel:Set("🔍 COİN ARANIYOR...")
                        task.wait(1)
                    end
                else
                    -- Hedef coin sayısına ulaşıldı
                    MurderMode = true
                    StatusLabel:Set("🎯 " .. TargetCoins .. " COİN TAMAMLANDI! Katil modu bekleniyor...")
                end
            end
            
            task.wait(0.5)
        end
        
        -- Sistem durduruldu
        StatusLabel:Set("🔴 Sistem: DURDU")
        SystemStatusLabel:Set("🔴 Sistem: DURDU")
    end)()
end

-- ============================
-- 🎮 BAŞLANGIÇ AYARLARI
-- ============================

-- Varsayılan ayarlar
getgenv().FarmSpeed = 0.3
getgenv().FarmRange = 100
getgenv().KillRange = 15
getgenv().KillCooldown = 1
getgenv().RetryFailed = true
getgenv().MaxRetries = 3
getgenv().VisualEffects = true
getgenv().Notifications = true

-- Başlangıç bildirimi
Rayfield:Notify({
    Title = "🤖 Akıllı FarmBot Yüklendi!",
    Content = "Özellikler:\n1. 50 coin otomatik topla\n2. Katil olunca herkesi öldür\n3. Başarısızları tekrar dene\n\n🎮 İyi oyunlar!",
    Duration = 8,
    Image = 4483362458
})

-- Otomatik güncelleme
RunService.Heartbeat:Connect(function()
    if AutoFarm then
        local players = GetAllPlayers()
        PlayersLeftLabel:Set("👥 Kalan Oyuncu: " .. #players)
        
        if MurderMode then
            ModeLabel:Set("🎯 Mod: KATİL MODU (" .. #players .. " kişi kaldı)")
        else
            local progressPercent = math.floor((CollectedCoins / TargetCoins) * 100)
            ModeLabel:Set("🎯 Mod: FARM MODU (%" .. progressPercent .. " tamamlandı)")
        end
    end
end)

-- Katil kontrolü
RunService.Heartbeat:Connect(function()
    IsMurderer = CheckIfMurderer()
end)

print("========================================")
print("🤖 AKILLI FARM + MURDER SCRIPT YÜKLENDİ")
print("🎯 Özellik: 50 Coin → Tüm Katil")
print("🖥️ GUI: Rayfield (Modern Arayüz)")
print("📞 Yardım: AI Assistant")
print("========================================")
