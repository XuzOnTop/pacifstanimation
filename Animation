--[[Fe anim class created by Brannon1964802.]]--

for i,v in next, game:GetService("Players").LocalPlayer.Character:GetDescendants() do if v:IsA("BasePart") and v.Name ~="HumanoidRootPart" then game:GetService("RunService").Heartbeat:connect(function() v.Velocity = Vector3.new(-30,0,0) end) end end game:GetService("StarterGui"):SetCore("SendNotification", { Title = "Im Patrick"; Text = "Netless Loaded!"; Icon = "rbxthumb://type=Asset&id=5107182114&w=150&h=150"}) Duration = 16;


local Vector3_101 = Vector3.new(1, 0, 1)
local netless_Y = Vector3.new(0, 25.1, 0)
local function getNetlessVelocity(realPartVelocity) --change this if you have a better method
    local mag = realPartVelocity.Magnitude
    if (mag > 1) and (mag < 100) then
        local unit = realPartVelocity.Unit
        if (unit.Y > 0.25) or (unit.Y < -0.75) then
            return realPartVelocity * (25.1 / realPartVelocity.Y)
        end
        realPartVelocity = unit * 100
    end
    return (realPartVelocity * Vector3_101) + netless_Y
end
local simradius = "shp" --simulation radius (net bypass) method
--"shp" - sethiddenproperty
--"ssr" - setsimulationradius
--false - disable
local noclipAllParts = true --set it to true if you want noclip
local antiragdoll = true --removes hingeConstraints and ballSocketConstraints from your character
local newanimate = true --disables the animate script and enables after reanimation
local discharscripts = true --disables all localScripts parented to your character before reanimation
local R15toR6 = true --tries to convert your character to r6 if its r15
local hatcollide = false --makes hats cancollide (credit to ShownApe) (works only with reanimate method 0)
local humState16 = true --enables collisions for limbs before the humanoid dies (using hum:ChangeState)
local addtools = false --puts all tools from backpack to character and lets you hold them after reanimation
local hedafterneck = true --disable aligns for head and enable after neck or torso is removed
local loadtime = game:GetService("Players").RespawnTime + 0.5 --anti respawn delay
local method = 3 --reanimation method
--methods:
--0 - breakJoints (takes [loadtime] seconds to laod)
--1 - limbs
--2 - limbs + anti respawn
--3 - limbs + breakJoints after [loadtime] seconds
--4 - remove humanoid + breakJoints
--5 - remove humanoid + limbs
local alignmode = 2 --AlignPosition mode
--modes:
--1 - AlignPosition rigidity enabled true
--2 - 2 AlignPositions rigidity enabled both true and false
--3 - AlignPosition rigidity enabled false
local flingpart = "HumanoidRootPart" --name of the part or the hat used for flinging
--the fling function
--usage: fling(target, duration, velocity)
--target can be set to: basePart, CFrame, Vector3, character model or humanoid (flings at mouse.Hit if argument not provided))
--duration (fling time in seconds) can be set to: a number or a string convertable to the number (0.5s if not provided),
--velocity (fling part rotation velocity) can be set to a vector3 value (Vector3.new(20000, 20000, 20000) if not provided)

local lp = game:GetService("Players").LocalPlayer
local rs = game:GetService("RunService")
local stepped = rs.Stepped
local heartbeat = rs.Heartbeat
local renderstepped = rs.RenderStepped
local sg = game:GetService("StarterGui")
local ws = game:GetService("Workspace")
local cf = CFrame.new
local v3 = Vector3.new
local v3_0 = Vector3.zero
local inf = math.huge

local c = lp.Character

if not (c and c.Parent) then
	return
end

c:GetPropertyChangedSignal("Parent"):Connect(function()
    if not (c and c.Parent) then
	    c = nil
	end
end)

local function gp(parent, name, className)
	if typeof(parent) == "Instance" then
		for i, v in pairs(parent:GetChildren()) do
			if (v.Name == name) and v:IsA(className) then
				return v
			end
		end
	end
	return nil
end

if type(getNetlessVelocity) ~= "function" then
    getNetlessVelocity = nil
end

local function align(Part0, Part1)
	Part0.CustomPhysicalProperties = PhysicalProperties.new(0, 0, 0, 0, 0)

	local att0 = Instance.new("Attachment")
	att0.Orientation = v3_0
	att0.Position = v3_0
	att0.Name = "att0_" .. Part0.Name
	local att1 = Instance.new("Attachment")
	att1.Orientation = v3_0
	att1.Position = v3_0
	att1.Name = "att1_" .. Part1.Name

	if (alignmode == 1) or (alignmode == 2) then
		local ape = Instance.new("AlignPosition", att0)
		ape.ApplyAtCenterOfMass = false
		ape.MaxForce = inf
		ape.MaxVelocity = inf
		ape.ReactionForceEnabled = false
		ape.Responsiveness = 200
		ape.Attachment1 = att1
		ape.Attachment0 = att0
		ape.Name = "AlignPositionRtrue"
		ape.RigidityEnabled = true
	end

	if (alignmode == 2) or (alignmode == 3) then
		local apd = Instance.new("AlignPosition", att0)
		apd.ApplyAtCenterOfMass = false
		apd.MaxForce = inf
		apd.MaxVelocity = inf
		apd.ReactionForceEnabled = false
		apd.Responsiveness = 200
		apd.Attachment1 = att1
		apd.Attachment0 = att0
		apd.Name = "AlignPositionRfalse"
		apd.RigidityEnabled = false
	end

	local ao = Instance.new("AlignOrientation", att0)
	ao.MaxAngularVelocity = inf
	ao.MaxTorque = inf
	ao.PrimaryAxisOnly = false
	ao.ReactionTorqueEnabled = false
	ao.Responsiveness = 200
	ao.Attachment1 = att1
	ao.Attachment0 = att0
	ao.RigidityEnabled = false

	if getNetlessVelocity then
	    local vel = Part0.Velocity
	    local velpart = Part1
        local rsteppedcon = renderstepped:Connect(function()
            Part0.Velocity = vel
        end)
        local heartbeatcon = heartbeat:Connect(function()
            vel = Part0.Velocity
            Part0.Velocity = getNetlessVelocity(velpart.Velocity)
        end)
        local attcon = nil
        Part0:GetPropertyChangedSignal("Parent"):Connect(function()
            if not (Part0 and Part0.Parent) then
                rsteppedcon:Disconnect()
                heartbeatcon:Disconnect()
                attcon:Disconnect()
            end
        end)
        attcon = att1:GetPropertyChangedSignal("Parent"):Connect(function()
	        if not (att1 and att1.Parent) then
	            attcon:Disconnect()
                velpart = Part0
	        else
	            velpart = att1.Parent
	            if not velpart:IsA("BasePart") then
	                velpart = Part0
	            end
	        end
	    end)
	end
	
	att0.Parent = Part0
    att1.Parent = Part1
end

local function respawnrequest()
	local ccfr = ws.CurrentCamera.CFrame
	local c = lp.Character
	lp.Character = nil
	lp.Character = c
	local con = nil
	con = ws.CurrentCamera.Changed:Connect(function(prop)
	    if (prop ~= "Parent") and (prop ~= "CFrame") then
	        return
	    end
	    ws.CurrentCamera.CFrame = ccfr
	    con:Disconnect()
    end)
end

local destroyhum = (method == 4) or (method == 5)
local breakjoints = (method == 0) or (method == 4)
local antirespawn = (method == 0) or (method == 2) or (method == 3)

hatcollide = hatcollide and (method == 0)

addtools = addtools and gp(lp, "Backpack", "Backpack")

local fenv = getfenv()
local shp = fenv.sethiddenproperty or fenv.set_hidden_property or fenv.set_hidden_prop or fenv.sethiddenprop
local ssr = fenv.setsimulationradius or fenv.set_simulation_radius or fenv.set_sim_radius or fenv.setsimradius or fenv.set_simulation_rad or fenv.setsimulationrad

if shp and (simradius == "shp") then
	spawn(function()
		while c and heartbeat:Wait() do
			shp(lp, "SimulationRadius", inf)
		end
	end)
elseif ssr and (simradius == "ssr") then
	spawn(function()
		while c and heartbeat:Wait() do
			ssr(inf)
		end
	end)
end

antiragdoll = antiragdoll and function(v)
	if v:IsA("HingeConstraint") or v:IsA("BallSocketConstraint") then
		v.Parent = nil
	end
end

if antiragdoll then
	for i, v in pairs(c:GetDescendants()) do
		antiragdoll(v)
	end
	c.DescendantAdded:Connect(antiragdoll)
end

if antirespawn then
	respawnrequest()
end

if method == 0 then
	wait(loadtime)
	if not c then
		return
	end
end

if discharscripts then
	for i, v in pairs(c:GetChildren()) do
		if v:IsA("LocalScript") then
			v.Disabled = true
		end
	end
elseif newanimate then
	local animate = gp(c, "Animate", "LocalScript")
	if animate and (not animate.Disabled) then
		animate.Disabled = true
	else
		newanimate = false
	end
end

if addtools then
	for i, v in pairs(addtools:GetChildren()) do
		if v:IsA("Tool") then
			v.Parent = c
		end
	end
end

pcall(function()
	settings().Physics.AllowSleep = false
	settings().Physics.PhysicsEnvironmentalThrottle = Enum.EnviromentalPhysicsThrottle.Disabled
end)

local OLDscripts = {}

for i, v in pairs(c:GetDescendants()) do
	if v.ClassName == "Script" then
		table.insert(OLDscripts, v)
	end
end

local scriptNames = {}

for i, v in pairs(c:GetDescendants()) do
	if v:IsA("BasePart") then
		local newName = tostring(i)
		local exists = true
		while exists do
			exists = false
			for i, v in pairs(OLDscripts) do
				if v.Name == newName then
					exists = true
				end
			end
			if exists then
				newName = newName .. "_"    
			end
		end
		table.insert(scriptNames, newName)
		Instance.new("Script", v).Name = newName
	end
end

c.Archivable = true
local hum = c:FindFirstChildOfClass("Humanoid")
if hum then
	for i, v in pairs(hum:GetPlayingAnimationTracks()) do
		v:Stop()
	end
end
local cl = c:Clone()
if hum and humState16 then
    hum:ChangeState(Enum.HumanoidStateType.Physics)
    if destroyhum then
        wait(1.6)
    end
end
if hum and hum.Parent and destroyhum then
    hum:Destroy()
end

if not c then
    return
end

local head = gp(c, "Head", "BasePart")
local torso = gp(c, "Torso", "BasePart") or gp(c, "UpperTorso", "BasePart")
local root = gp(c, "HumanoidRootPart", "BasePart")
if hatcollide and c:FindFirstChildOfClass("Accessory") then
    local anything = c:FindFirstChildOfClass("BodyColors") or gp(c, "Health", "Script")
    if not (torso and root and anything) then
        return
    end
    torso:Destroy()
    root:Destroy()
    if shp then
        for i,v in pairs(c:GetChildren()) do
            if v:IsA("Accessory") then
                shp(v, "BackendAccoutrementState", 0)
            end 
        end
    end
    anything:Destroy()
end

local model = Instance.new("Model", c)
model.Name = model.ClassName

model:GetPropertyChangedSignal("Parent"):Connect(function()
    if not (model and model.Parent) then
	    model = nil
    end
end)

for i, v in pairs(c:GetChildren()) do
	if v ~= model then
		if addtools and v:IsA("Tool") then
			for i1, v1 in pairs(v:GetDescendants()) do
				if v1 and v1.Parent and v1:IsA("BasePart") then
					local bv = Instance.new("BodyVelocity", v1)
					bv.Velocity = v3_0
					bv.MaxForce = v3(1000, 1000, 1000)
					bv.P = 1250
					bv.Name = "bv_" .. v.Name
				end
			end
		end
		v.Parent = model
	end
end

if breakjoints then
	model:BreakJoints()
else
	if head and torso then
		for i, v in pairs(model:GetDescendants()) do
			if v:IsA("Weld") or v:IsA("Snap") or v:IsA("Glue") or v:IsA("Motor") or v:IsA("Motor6D") then
				local save = false
				if (v.Part0 == torso) and (v.Part1 == head) then
					save = true
				end
				if (v.Part0 == head) and (v.Part1 == torso) then
					save = true
				end
				if save then
					if hedafterneck then
						hedafterneck = v
					end
				else
					v:Destroy()
				end
			end
		end
	end
	if method == 3 then
		spawn(function()
			wait(loadtime)
			if model then
				model:BreakJoints()
			end
		end)
	end
end

cl.Parent = c
for i, v in pairs(cl:GetChildren()) do
	v.Parent = c
end
cl:Destroy()

local noclipmodel = (noclipAllParts and c) or model
local noclipcon = nil
local function uncollide()
	if noclipmodel then
		for i, v in pairs(noclipmodel:GetDescendants()) do
		    if v:IsA("BasePart") then
			    v.CanCollide = false
		    end
		end
	else
		noclipcon:Disconnect()
	end
end
noclipcon = stepped:Connect(uncollide)
uncollide()

for i, scr in pairs(model:GetDescendants()) do
	if (scr.ClassName == "Script") and table.find(scriptNames, scr.Name) then
		local Part0 = scr.Parent
		if Part0:IsA("BasePart") then
			for i1, scr1 in pairs(c:GetDescendants()) do
				if (scr1.ClassName == "Script") and (scr1.Name == scr.Name) and (not scr1:IsDescendantOf(model)) then
					local Part1 = scr1.Parent
					if (Part1.ClassName == Part0.ClassName) and (Part1.Name == Part0.Name) then
						align(Part0, Part1)
						scr:Destroy()
						scr1:Destroy()
						break
					end
				end
			end
		end
	end
end

for i, v in pairs(c:GetDescendants()) do
	if v and v.Parent and (not v:IsDescendantOf(model)) then
		if v:IsA("Decal") then
		    v.Transparency = 1
		elseif v:IsA("BasePart") then
			v.Transparency = 1
			v.Anchored = false
		elseif v:IsA("ForceField") then
			v.Visible = false
		elseif v:IsA("Sound") then
			v.Playing = false
		elseif v:IsA("BillboardGui") or v:IsA("SurfaceGui") or v:IsA("ParticleEmitter") or v:IsA("Fire") or v:IsA("Smoke") or v:IsA("Sparkles") then
			v.Enabled = false
		end
	end
end

if newanimate then
	local animate = gp(c, "Animate", "LocalScript")
	if animate then
		animate.Disabled = false
	end
end

if addtools then
	for i, v in pairs(c:GetChildren()) do
		if v:IsA("Tool") then
			v.Parent = addtools
		end
	end
end

local hum0 = model:FindFirstChildOfClass("Humanoid")
if hum0 then
    hum0:GetPropertyChangedSignal("Parent"):Connect(function()
        if not (hum0 and hum0.Parent) then
            hum0 = nil
        end
    end)
end

local hum1 = c:FindFirstChildOfClass("Humanoid")
if hum1 then
    hum1:GetPropertyChangedSignal("Parent"):Connect(function()
        if not (hum1 and hum1.Parent) then
            hum1 = nil
        end
    end)
    
	ws.CurrentCamera.CameraSubject = hum1
	local camSubCon = nil
	local function camSubFunc()
		camSubCon:Disconnect()
		if c and hum1 then
			ws.CurrentCamera.CameraSubject = hum1
		end
	end
	camSubCon = renderstepped:Connect(camSubFunc)
	if hum0 then
		hum0:GetPropertyChangedSignal("Jump"):Connect(function()
			if hum1 then
				hum1.Jump = hum0.Jump
			end
		end)
	else
		respawnrequest()
	end
end

local rb = Instance.new("BindableEvent", c)
rb.Event:Connect(function()
	rb:Destroy()
	sg:SetCore("ResetButtonCallback", true)
	if destroyhum then
		c:BreakJoints()
		return
	end
	if hum0 and (hum0.Health > 0) then
		model:BreakJoints()
		hum0.Health = 0
	end
	if antirespawn then
	    respawnrequest()
	end
end)
sg:SetCore("ResetButtonCallback", rb)

spawn(function()
	while c do
		if hum0 and hum1 then
			hum1.Jump = hum0.Jump
		end
		wait()
	end
	sg:SetCore("ResetButtonCallback", true)
end)

R15toR6 = R15toR6 and hum1 and (hum1.RigType == Enum.HumanoidRigType.R15)
if R15toR6 then
    local part = gp(c, "HumanoidRootPart", "BasePart") or gp(c, "UpperTorso", "BasePart") or gp(c, "LowerTorso", "BasePart") or gp(c, "Head", "BasePart") or c:FindFirstChildWhichIsA("BasePart")
	if part then
	    local cfr = part.CFrame
		local R6parts = { 
			head = {
				Name = "Head",
				Size = v3(2, 1, 1),
				R15 = {
					Head = 0
				}
			},
			torso = {
				Name = "Torso",
				Size = v3(2, 2, 1),
				R15 = {
					UpperTorso = 0.2,
					LowerTorso = -0.8
				}
			},
			root = {
				Name = "HumanoidRootPart",
				Size = v3(2, 2, 1),
				R15 = {
					HumanoidRootPart = 0
				}
			},
			leftArm = {
				Name = "Left Arm",
				Size = v3(1, 2, 1),
				R15 = {
					LeftHand = -0.849,
					LeftLowerArm = -0.174,
					LeftUpperArm = 0.415
				}
			},
			rightArm = {
				Name = "Right Arm",
				Size = v3(1, 2, 1),
				R15 = {
					RightHand = -0.849,
					RightLowerArm = -0.174,
					RightUpperArm = 0.415
				}
			},
			leftLeg = {
				Name = "Left Leg",
				Size = v3(1, 2, 1),
				R15 = {
					LeftFoot = -0.85,
					LeftLowerLeg = -0.29,
					LeftUpperLeg = 0.49
				}
			},
			rightLeg = {
				Name = "Right Leg",
				Size = v3(1, 2, 1),
				R15 = {
					RightFoot = -0.85,
					RightLowerLeg = -0.29,
					RightUpperLeg = 0.49
				}
			}
		}
		for i, v in pairs(c:GetChildren()) do
			if v:IsA("BasePart") then
				for i1, v1 in pairs(v:GetChildren()) do
					if v1:IsA("Motor6D") then
						v1.Part0 = nil
					end
				end
			end
		end
		part.Archivable = true
		for i, v in pairs(R6parts) do
			local part = part:Clone()
			part:ClearAllChildren()
			part.Name = v.Name
			part.Size = v.Size
			part.CFrame = cfr
			part.Anchored = false
			part.Transparency = 1
			part.CanCollide = false
			for i1, v1 in pairs(v.R15) do
				local R15part = gp(c, i1, "BasePart")
				local att = gp(R15part, "att1_" .. i1, "Attachment")
				if R15part then
					local weld = Instance.new("Weld", R15part)
					weld.Name = "Weld_" .. i1
					weld.Part0 = part
					weld.Part1 = R15part
					weld.C0 = cf(0, v1, 0)
					weld.C1 = cf(0, 0, 0)
					R15part.Massless = true
					R15part.Name = "R15_" .. i1
					R15part.Parent = part
					if att then
						att.Parent = part
						att.Position = v3(0, v1, 0)
					end
				end
			end
			part.Parent = c
			R6parts[i] = part
		end
		local R6joints = {
			neck = {
				Parent = R6parts.torso,
				Name = "Neck",
				Part0 = R6parts.torso,
				Part1 = R6parts.head,
				C0 = cf(0, 1, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0),
				C1 = cf(0, -0.5, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0)
			},
			rootJoint = {
				Parent = R6parts.root,
				Name = "RootJoint" ,
				Part0 = R6parts.root,
				Part1 = R6parts.torso,
				C0 = cf(0, 0, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0),
				C1 = cf(0, 0, 0, -1, 0, 0, 0, 0, 1, 0, 1, -0)
			},
			rightShoulder = {
				Parent = R6parts.torso,
				Name = "Right Shoulder",
				Part0 = R6parts.torso,
				Part1 = R6parts.rightArm,
				C0 = cf(1, 0.5, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0),
				C1 = cf(-0.5, 0.5, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0)
			},
			leftShoulder = {
				Parent = R6parts.torso,
				Name = "Left Shoulder",
				Part0 = R6parts.torso,
				Part1 = R6parts.leftArm,
				C0 = cf(-1, 0.5, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0),
				C1 = cf(0.5, 0.5, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			},
			rightHip = {
				Parent = R6parts.torso,
				Name = "Right Hip",
				Part0 = R6parts.torso,
				Part1 = R6parts.rightLeg,
				C0 = cf(1, -1, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0),
				C1 = cf(0.5, 1, 0, 0, 0, 1, 0, 1, -0, -1, 0, 0)
			},
			leftHip = {
				Parent = R6parts.torso,
				Name = "Left Hip" ,
				Part0 = R6parts.torso,
				Part1 = R6parts.leftLeg,
				C0 = cf(-1, -1, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0),
				C1 = cf(-0.5, 1, 0, 0, 0, -1, 0, 1, 0, 1, 0, 0)
			}
		}
		for i, v in pairs(R6joints) do
			local joint = Instance.new("Motor6D")
			for prop, val in pairs(v) do
				joint[prop] = val
			end
			R6joints[i] = joint
		end
		if hum1 then
    		hum1.RigType = Enum.HumanoidRigType.R6
    		hum1.HipHeight = 0
		end
	end
end

local torso1 = torso
torso = gp(c, "Torso", "BasePart") or ((not R15toR6) and gp(c, torso.Name, "BasePart"))
if (typeof(hedafterneck) == "Instance") and head and torso and torso1 then
	local conNeck = nil
	local conTorso = nil
	local contorso1 = nil
	local aligns = {}
	local function enableAligns()
	    conNeck:Disconnect()
        conTorso:Disconnect()
        conTorso1:Disconnect()
		for i, v in pairs(aligns) do
			v.Enabled = true
		end
	end
	conNeck = hedafterneck.Changed:Connect(function(prop)
	    if table.find({"Part0", "Part1", "Parent"}, prop) then
	        enableAligns()
		end
	end)
	conTorso = torso:GetPropertyChangedSignal("Parent"):Connect(enableAligns)
	conTorso1 = torso1:GetPropertyChangedSignal("Parent"):Connect(enableAligns)
	for i, v in pairs(head:GetDescendants()) do
		if v:IsA("AlignPosition") or v:IsA("AlignOrientation") then
			i = tostring(i)
			aligns[i] = v
			v:GetPropertyChangedSignal("Parent"):Connect(function()
			    aligns[i] = nil
			end)
			v.Enabled = false
		end
	end
end

local flingpart0 = gp(model, flingpart, "BasePart") or gp(gp(model, flingpart, "Accessory"), "Handle", "BasePart")
local flingpart1 = gp(c, flingpart, "BasePart") or gp(gp(c, flingpart, "Accessory"), "Handle", "BasePart")

local fling = function() end
if flingpart0 and flingpart1 then
    flingpart0:GetPropertyChangedSignal("Parent"):Connect(function()
        if not (flingpart0 and flingpart0.Parent) then
            flingpart0 = nil
            fling = function() end
        end
    end)
    flingpart0.Archivable = true
    flingpart1:GetPropertyChangedSignal("Parent"):Connect(function()
        if not (flingpart1 and flingpart1.Parent) then
            flingpart1 = nil
            fling = function() end
        end
    end)
    local att0 = gp(flingpart0, "att0_" .. flingpart0.Name, "Attachment")
    local att1 = gp(flingpart1, "att1_" .. flingpart1.Name, "Attachment")
    if att0 and att1 then
        att0:GetPropertyChangedSignal("Parent"):Connect(function()
            if not (att0 and att0.Parent) then
                att0 = nil
                fling = function() end
            end
        end)
        att1:GetPropertyChangedSignal("Parent"):Connect(function()
            if not (att1 and att1.Parent) then
                att1 = nil
                fling = function() end
            end
        end)
        local lastfling = nil
        local mouse = lp:GetMouse()
        fling = function(target, duration, rotVelocity)
            if typeof(target) == "Instance" then
                if target:IsA("BasePart") then
                    target = target.Position
                elseif target:IsA("Model") then
                    target = gp(target, "HumanoidRootPart", "BasePart") or gp(target, "Torso", "BasePart") or gp(target, "UpperTorso", "BasePart") or target:FindFirstChildWhichIsA("BasePart")
                    if target then
                        target = target.Position
                    else
                        return
                    end
                elseif target:IsA("Humanoid") then
                    local parent = target.Parent
                    if not (parent and parent:IsA("Model")) then
                        return
                    end
                    target = gp(target, "HumanoidRootPart", "BasePart") or gp(target, "Torso", "BasePart") or gp(target, "UpperTorso", "BasePart") or target:FindFirstChildWhichIsA("BasePart")
                    if target then
                        target = target.Position
                    else
                        return
                    end
                else
                    return
                end
            elseif typeof(target) == "CFrame" then
                target = target.Position
            elseif typeof(target) ~= "Vector3" then
                target = mouse.Hit
                if target then
                    target = target.Position
                else
                    return
                end
            end
            lastfling = target
            if type(duration) ~= "number" then
                duration = tonumber(duration) or 0.5
            end
            if typeof(rotVelocity) ~= "Vector3" then
                rotVelocity = v3(20000, 20000, 20000)
            end
            if not (target and flingpart0 and flingpart1 and att0 and att1) then
                return
            end
            local flingpart = flingpart0:Clone()
            flingpart.Transparency = 1
            flingpart.Size = v3(0.01, 0.01, 0.01)
            flingpart.CanCollide = false
            flingpart.Name = "flingpart_" .. flingpart0.Name
            flingpart.Anchored = true
            flingpart.Velocity = v3_0
            flingpart.RotVelocity = v3_0
            flingpart:GetPropertyChangedSignal("Parent"):Connect(function()
                if not (flingpart and flingpart.Parent) then
                    flingpart = nil
                end
            end)
            flingpart.Parent = flingpart1
            if flingpart0.Transparency > 0.5 then
                flingpart0.Transparency = 0.5
            end
            att1.Parent = flingpart
            for i, v in pairs(att0:GetChildren()) do
                if v:IsA("AlignOrientation") then
                    v.Enabled = false
                end
            end
            local con = nil
            con = heartbeat:Connect(function()
                if target and (lastfling == target) and flingpart and flingpart0 and flingpart1 and att0 and att1 then
                    flingpart0.RotVelocity = rotVelocity
                    flingpart.Position = target
                else
                    con:Disconnect()
                end
            end)
            local rsteppedRotVel = v3(
                ((rotVelocity.X > 0) and -1) or 1,
                ((rotVelocity.Y > 0) and -1) or 1,
                ((rotVelocity.Z > 0) and -1) or 1
            )
            local con = nil
            con = renderstepped:Connect(function()
                if target and (lastfling == target) and flingpart and flingpart0 and flingpart1 and att0 and att1 then
                    flingpart0.RotVelocity = rsteppedRotVel
                    flingpart.Position = target
                else
                    con:Disconnect()
                end
            end)
            wait(duration)
            if lastfling ~= target then
                if flingpart then
                    if att1 and (att1.Parent == flingpart) then
                        att1.Parent = flingpart1
                    end
                    flingpart:Destroy()
                end
                return
            end
            target = nil
            if not (flingpart and flingpart0 and flingpart1 and att0 and att1) then
                return
            end
            flingpart0.RotVelocity = v3_0
            att1.Parent = flingpart1
            for i, v in pairs(att0:GetChildren()) do
                if v:IsA("AlignOrientation") then
                    v.Enabled = true
                end
            end
            if flingpart then
                flingpart:Destroy()
            end
        end
    end
end

Player=game:GetService("Players").LocalPlayer
Character=Player.Character 
PlayerGui=Player.PlayerGui
Backpack=Player.Backpack 
Torso=Character.Torso 
Head=Character.Head 
Humanoid=Character.Humanoid
m=Instance.new('Model',Character)
LeftArm=Character["Left Arm"] 
LeftLeg=Character["Left Leg"] 
RightArm=Character["Right Arm"] 
RightLeg=Character["Right Leg"] 
LS=Torso["Left Shoulder"] 
LH=Torso["Left Hip"] 
RS=Torso["Right Shoulder"] 
RH=Torso["Right Hip"] 
Face = Head.face
Neck=Torso.Neck
--it=Instance.new
attacktype=1
vt=Vector3.new
cf=CFrame.new
euler=CFrame.fromEulerAnglesXYZ
angles=CFrame.Angles
cloaked=false
necko=cf(0, 1, 0, -1, -0, -0, 0, 0, 1, 0, 1, 0)
necko2=cf(0, -0.5, 0, -1, -0, -0, 0, 0, 1, 0, 1, 0)
LHC0=cf(-1,-1,0,-0,-0,-1,0,1,0,1,0,0)
LHC1=cf(-0.5,1,0,-0,-0,-1,0,1,0,1,0,0)
RHC0=cf(1,-1,0,0,0,1,0,1,0,-1,-0,-0)
RHC1=cf(0.5,1,0,0,0,1,0,1,0,-1,-0,-0)
RootPart=Character.HumanoidRootPart
RootJoint=RootPart.RootJoint
RootCF=euler(-1.57,0,3.14)
attack = false 
attackdebounce = false 
deb=false
equipped=true
hand=false
MMouse=nil
combo=0
mana=0
trispeed=.2
attackmode='none'
local idle=0
local Anim="Idle"
local gun=false
local shoot=false
player=nil 
mana=0

local defensevalue = .3
local speedvalue = 1
local damagevalue = 1
local cf = CFrame.new-- make things easier :)
local mr = math.rad
local angles = CFrame.Angles
local ud = UDim2.new
local c3 = Color3.new

local stats=Instance.new('Folder',Character)
stats.Name='Stats'
local block=Instance.new('BoolValue',stats)
block.Name='Block'
block.Value=false
local stun=Instance.new('BoolValue',stats)
stun.Name='Stun'
stun.Value=false
local defense=Instance.new('NumberValue',stats)
defense.Name='Defence'
defense.Value=defensevalue
local speed=Instance.new('NumberValue',stats)
speed.Name='Speed'
speed.Value=speedvalue
local damagea=Instance.new('NumberValue',stats)
damagea.Name='Damage'
damagea.Value=damagevalue

Humanoid = Character.Humanoid
if Humanoid:FindFirstChild("Animator")then
Humanoid:FindFirstChild("Animator"):Destroy()
end

Humanoid.WalkSpeed = 3

--[[music = Instance.new("Sound", Torso)
music.SoundId = "http://www.roblox.com/asset/?id=179604943"
music.Volume = 0.5
music.Looped = true
wait(0.1)
music:Play()]]--

Colorpart1 = Torso.BrickColor.r
Colorpart2 = Torso.BrickColor.g
Colorpart3 = Torso.BrickColor.b

CloakEffectLight2 = Instance.new("PointLight", Torso)
CloakEffectLight2.Color = Color3.new(Colorpart1, Colorpart2, Colorpart3)
CloakEffectLight2.Range = 7.5
CloakEffectLight2.Brightness = 7.5
CloakEffectLight2.Enabled = true

mouse=Player:GetMouse()
--save shoulders 
RSH, LSH=nil, nil 
--welds 
RW, LW=Instance.new("Motor"), Instance.new("Motor") 
RW.Name="Right Shoulder" LW.Name="Left Shoulder"
LH=Torso["Left Hip"]
RH=Torso["Right Hip"]
TorsoColor=Torso.BrickColor
function NoOutline(Part)
Part.TopSurface,Part.BottomSurface,Part.LeftSurface,Part.RightSurface,Part.FrontSurface,Part.BackSurface = 10,10,10,10,10,10
end
player=Player 
ch=Character
RSH=ch.Torso["Right Shoulder"] 
LSH=ch.Torso["Left Shoulder"] 
-- 
RSH.Parent=nil 
LSH.Parent=nil 
-- 
RW.Name="Right Shoulder"
RW.Part0=ch.Torso 
RW.C0=cf(1.5, 0.5, 0) --* CFrame.fromEulerAnglesXYZ(1.3, 0, -0.5) 
RW.C1=cf(0, 0.5, 0) 
RW.Part1=ch["Right Arm"] 
RW.Parent=ch.Torso 
-- 
LW.Name="Left Shoulder"
LW.Part0=ch.Torso 
LW.C0=cf(-1.5, 0.5, 0) --* CFrame.fromEulerAnglesXYZ(1.7, 0, 0.8) 
LW.C1=cf(0, 0.5, 0) 
LW.Part1=ch["Left Arm"] 
LW.Parent=ch.Torso 

	local function weldBetween(a, b)
	    local weldd = Instance.new("ManualWeld")
	    weldd.Part0 = a
	    weldd.Part1 = b
	    weldd.C0 = CFrame.new()
	    weldd.C1 = b.CFrame:inverse() * a.CFrame
	    weldd.Parent = a
	    return weldd
	end
	
fat = Instance.new("BindableEvent", script)
fat.Name = "Heartbeat"

script:WaitForChild("Heartbeat")

frame = 1 / 30
tf = 0
allowframeloss = false --if set to true will fire every frame it possibly can. This will result in multiple events happening at the same time whenever delta returns frame*2 or greater.
tossremainder = false --if set to true t will be set to 0 after Fire()-ing.
lastframe = tick()
script.Heartbeat:Fire() --ayy lmao

game:GetService("RunService").Heartbeat:connect(function(s, p) --herp derp
	tf = tf + s
	if tf >= frame then
		if allowframeloss then
			script.Heartbeat:Fire()
			lastframe = tick()
		else
--print("FIRED "..math.floor(t/frame).." FRAME(S)","REMAINDER "..(t - frame*(math.floor(t/frame))))
			for i = 1, math.floor(tf / frame) do
				script.Heartbeat:Fire()
			end
			lastframe = tick()
		end
		if tossremainder then
			tf = 0
		else
			tf = tf - frame * math.floor(tf / frame)
		end
	end
end)

--To use: fat.Event:fat.Event:wait() or fat.Event:connect(function() asdcode end)

local function CFrameFromTopBack(at, top, back)
local right = top:Cross(back)
return CFrame.new(at.x, at.y, at.z,
right.x, top.x, back.x,
right.y, top.y, back.y,
right.z, top.z, back.z)
end

function Triangle(a, b, c)
local edg1 = (c-a):Dot((b-a).unit)
local edg2 = (a-b):Dot((c-b).unit)
local edg3 = (b-c):Dot((a-c).unit)
if edg1 <= (b-a).magnitude and edg1 >= 0 then
a, b, c = a, b, c
elseif edg2 <= (c-b).magnitude and edg2 >= 0 then
a, b, c = b, c, a
elseif edg3 <= (a-c).magnitude and edg3 >= 0 then
a, b, c = c, a, b
else
assert(false, "unreachable")
end
 
local len1 = (c-a):Dot((b-a).unit)
local len2 = (b-a).magnitude - len1
local width = (a + (b-a).unit*len1 - c).magnitude
 
local maincf = CFrameFromTopBack(a, (b-a):Cross(c-b).unit, -(b-a).unit)
 
local list = {}

local TrailColor = ("Dark grey")
 
if len1 > 0.01 then
local w1 = Instance.new('WedgePart', m)
game:GetService("Debris"):AddItem(w1,5)
w1.Material = "SmoothPlastic"
w1.FormFactor = 'Custom'
w1.BrickColor = BrickColor.new(TrailColor)
w1.Transparency = 0
w1.Reflectance = 0
w1.Material = "SmoothPlastic"
w1.CanCollide = false
NoOutline(w1)
local sz = Vector3.new(0.2, width, len1)
w1.Size = sz
local sp = Instance.new("SpecialMesh",w1)
sp.MeshType = "Wedge"
sp.Scale = Vector3.new(0,1,1) * sz/w1.Size
w1:BreakJoints()
w1.Anchored = true
w1.Parent = workspace
w1.Transparency = 0.7
table.insert(Effects,{w1,"Disappear",.01})
w1.CFrame = maincf*CFrame.Angles(math.pi,0,math.pi/2)*CFrame.new(0,width/2,len1/2)
table.insert(list,w1)
end
 
if len2 > 0.01 then
local w2 = Instance.new('WedgePart', m)
game:GetService("Debris"):AddItem(w2,5)
w2.Material = "SmoothPlastic"
w2.FormFactor = 'Custom'
w2.BrickColor = BrickColor.new(TrailColor)
w2.Transparency = 0
w2.Reflectance = 0
w2.Material = "SmoothPlastic"
w2.CanCollide = false
NoOutline(w2)
local sz = Vector3.new(0.2, width, len2)
w2.Size = sz
local sp = Instance.new("SpecialMesh",w2)
sp.MeshType = "Wedge"
sp.Scale = Vector3.new(0,1,1) * sz/w2.Size
w2:BreakJoints()
w2.Anchored = true
w2.Parent = workspace
w2.Transparency = 0.7
table.insert(Effects,{w2,"Disappear",.01})
w2.CFrame = maincf*CFrame.Angles(math.pi,math.pi,-math.pi/2)*CFrame.new(0,width/2,-len1 - len2/2)
table.insert(list,w2)
end
return unpack(list)
end
	
function rayCast(Pos, Dir, Max, Ignore)  -- Origin Position , Direction, MaxDistance , IgnoreDescendants
return game:service("Workspace"):FindPartOnRay(Ray.new(Pos, Dir.unit * (Max or 999.999)), Ignore) 
end 	
 
function clerp(a,b,t) 
local qa = {QuaternionFromCFrame(a)}
local qb = {QuaternionFromCFrame(b)} 
local ax, ay, az = a.x, a.y, a.z 
local bx, by, bz = b.x, b.y, b.z
local _t = 1-t
return QuaternionToCFrame(_t*ax + t*bx, _t*ay + t*by, _t*az + t*bz,QuaternionSlerp(qa, qb, t)) 
end
 
function QuaternionFromCFrame(cf) 
local mx, my, mz, m00, m01, m02, m10, m11, m12, m20, m21, m22 = cf:components() 
local trace = m00 + m11 + m22 
if trace > 0 then 
local s = math.sqrt(1 + trace) 
local recip = 0.5/s 
return (m21-m12)*recip, (m02-m20)*recip, (m10-m01)*recip, s*0.5 
else 
local i = 0 
if m11 > m00 then
i = 1
end
if m22 > (i == 0 and m00 or m11) then 
i = 2 
end 
if i == 0 then 
local s = math.sqrt(m00-m11-m22+1) 
local recip = 0.5/s 
return 0.5*s, (m10+m01)*recip, (m20+m02)*recip, (m21-m12)*recip 
elseif i == 1 then 
local s = math.sqrt(m11-m22-m00+1) 
local recip = 0.5/s 
return (m01+m10)*recip, 0.5*s, (m21+m12)*recip, (m02-m20)*recip 
elseif i == 2 then 
local s = math.sqrt(m22-m00-m11+1) 
local recip = 0.5/s return (m02+m20)*recip, (m12+m21)*recip, 0.5*s, (m10-m01)*recip 
end 
end 
end
 
function QuaternionToCFrame(px, py, pz, x, y, z, w) 
local xs, ys, zs = x + x, y + y, z + z 
local wx, wy, wz = w*xs, w*ys, w*zs 
local xx = x*xs 
local xy = x*ys 
local xz = x*zs 
local yy = y*ys 
local yz = y*zs 
local zz = z*zs 
return CFrame.new(px, py, pz,1-(yy+zz), xy - wz, xz + wy,xy + wz, 1-(xx+zz), yz - wx, xz - wy, yz + wx, 1-(xx+yy)) 
end
 
function QuaternionSlerp(a, b, t) 
local cosTheta = a[1]*b[1] + a[2]*b[2] + a[3]*b[3] + a[4]*b[4] 
local startInterp, finishInterp; 
if cosTheta >= 0.0001 then 
if (1 - cosTheta) > 0.0001 then 
local theta = math.acos(cosTheta) 
local invSinTheta = 1/math.sin(theta) 
startInterp = math.sin((1-t)*theta)*invSinTheta 
finishInterp = math.sin(t*theta)*invSinTheta  
else 
startInterp = 1-t 
finishInterp = t 
end 
else 
if (1+cosTheta) > 0.0001 then 
local theta = math.acos(-cosTheta) 
local invSinTheta = 1/math.sin(theta) 
startInterp = math.sin((t-1)*theta)*invSinTheta 
finishInterp = math.sin(t*theta)*invSinTheta 
else 
startInterp = t-1 
finishInterp = t 
end 
end 
return a[1]*startInterp + b[1]*finishInterp, a[2]*startInterp + b[2]*finishInterp, a[3]*startInterp + b[3]*finishInterp, a[4]*startInterp + b[4]*finishInterp 
end

sitting=false
resting=false
meditating=false
sprint=false

mouse.Button1Down:connect(function()
end)
print'Only instinct left is survival.'
mouse.KeyDown:connect(function(k)
	k=k:lower()
	if k=='z' and attack==false and resting==false and sprint==false and meditating==false then
	attack=true
	if sitting==false then
	sitting=true
	Humanoid.WalkSpeed = 0
	for i=0,1,0.04 do
	fat.Event:wait()
	RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,-1.25)*angles(math.rad(0),math.rad(15),math.rad(0)),0.15)
	Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0)*angles(math.rad(20),math.rad(0),math.rad(30)),0.15)
	RW.C0=clerp(RW.C0,CFrame.new(1.5,0.5,0)*angles(math.rad(0),math.rad(0),math.rad(20)),0.15)
	LW.C0=clerp(LW.C0,CFrame.new(-1.5,0.3,0)*angles(math.rad(15),math.rad(0),math.rad(-25)),0.15)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(-60),math.rad(90),math.rad(0))*angles(math.rad(-2.5),math.rad(0),math.rad(0)),0.15)
	LH.C0=clerp(LH.C0,cf(-0.75,0.25,-1)*angles(math.rad(-15),math.rad(-90),math.rad(0))*angles(math.rad(-15),math.rad(0),math.rad(0)),0.15)
	end
	elseif sitting==true then
	sitting=false
	for i=0,1,0.04 do
	fat.Event:wait()
	RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,-1.25)*angles(math.rad(0),math.rad(15),math.rad(0)),0.15)
	Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0)*angles(math.rad(20),math.rad(0),math.rad(30)),0.15)
	RW.C0=clerp(RW.C0,CFrame.new(1.5,0.5,0)*angles(math.rad(0),math.rad(0),math.rad(20)),0.15)
	LW.C0=clerp(LW.C0,CFrame.new(-1.5,0.3,0)*angles(math.rad(15),math.rad(0),math.rad(-25)),0.15)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(-60),math.rad(90),math.rad(0))*angles(math.rad(-2.5),math.rad(0),math.rad(0)),0.15)
	LH.C0=clerp(LH.C0,cf(-0.75,0.25,-1)*angles(math.rad(-15),math.rad(-90),math.rad(0))*angles(math.rad(-15),math.rad(0),math.rad(0)),0.15)
	end
	Humanoid.WalkSpeed = 3
	end
	attack=false
	end
	if k=='x' and attack==false and sitting==false and sprint==false and meditating==false then
	attack=true
	if resting==false then
	resting=true
	Humanoid.WalkSpeed = 0
	for i=0,1,0.04 do
	fat.Event:wait()
	RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,-1.05)*angles(math.rad(0),math.rad(15),math.rad(0)),0.15)
	Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0)*angles(math.rad(20),math.rad(0),math.rad(30)),0.15)
	RW.C0=clerp(RW.C0,CFrame.new(1.5,0.5,0)*angles(math.rad(0),math.rad(0),math.rad(20)),0.15)
	LW.C0=clerp(LW.C0,CFrame.new(-1.5,0.1,0)*angles(math.rad(15),math.rad(0),math.rad(-25)),0.15)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(-40),math.rad(90),math.rad(0))*angles(math.rad(-2.5),math.rad(0),math.rad(0)),0.15)
	LH.C0=clerp(LH.C0,cf(-0.75,0.05,-1)*angles(math.rad(-15),math.rad(-90),math.rad(0))*angles(math.rad(-15),math.rad(0),math.rad(0)),0.15)
	end
	elseif resting==true then
	resting=false
	for i=0,1,0.04 do
	fat.Event:wait()
	RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,-1.05)*angles(math.rad(0),math.rad(15),math.rad(0)),0.15)
	Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0)*angles(math.rad(20),math.rad(0),math.rad(30)),0.15)
	RW.C0=clerp(RW.C0,CFrame.new(1.5,0.5,0)*angles(math.rad(0),math.rad(0),math.rad(20)),0.15)
	LW.C0=clerp(LW.C0,CFrame.new(-1.5,0.1,0)*angles(math.rad(15),math.rad(0),math.rad(-25)),0.15)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(-40),math.rad(90),math.rad(0))*angles(math.rad(-2.5),math.rad(0),math.rad(0)),0.15)
	LH.C0=clerp(LH.C0,cf(-0.75,0.05,-1)*angles(math.rad(-15),math.rad(-90),math.rad(0))*angles(math.rad(-15),math.rad(0),math.rad(0)),0.15)
	end
	Humanoid.WalkSpeed = 3
	end
	attack=false
	end
	if k=='c' and attack==false and sitting==false and resting==false and sprint==false then
	attack=true
	if meditating==false then
	meditating=true
	Humanoid.WalkSpeed = 0
	for i=0,1,0.04 do
	fat.Event:wait()
	RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,-1.05)*angles(math.rad(0),math.rad(15),math.rad(0)),0.15)
	Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0)*angles(math.rad(20),math.rad(0),math.rad(30)),0.15)
	RW.C0=clerp(RW.C0,CFrame.new(1.5,0.5,0)*angles(math.rad(0),math.rad(0),math.rad(20)),0.15)
	LW.C0=clerp(LW.C0,CFrame.new(-1.5,0.1,0)*angles(math.rad(15),math.rad(0),math.rad(-25)),0.15)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(-40),math.rad(90),math.rad(0))*angles(math.rad(-2.5),math.rad(0),math.rad(0)),0.15)
	LH.C0=clerp(LH.C0,cf(-0.75,0.05,-1)*angles(math.rad(-15),math.rad(-90),math.rad(0))*angles(math.rad(-15),math.rad(0),math.rad(0)),0.15)
	end
	elseif meditating==true then
	meditating=false
	for i=0,1,0.04 do
	fat.Event:wait()
	RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,-1.05)*angles(math.rad(0),math.rad(15),math.rad(0)),0.15)
	Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0)*angles(math.rad(20),math.rad(0),math.rad(30)),0.15)
	RW.C0=clerp(RW.C0,CFrame.new(1.5,0.5,0)*angles(math.rad(0),math.rad(0),math.rad(20)),0.15)
	LW.C0=clerp(LW.C0,CFrame.new(-1.5,0.1,0)*angles(math.rad(15),math.rad(0),math.rad(-25)),0.15)
	RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(-40),math.rad(90),math.rad(0))*angles(math.rad(-2.5),math.rad(0),math.rad(0)),0.15)
	LH.C0=clerp(LH.C0,cf(-0.75,0.05,-1)*angles(math.rad(-15),math.rad(-90),math.rad(0))*angles(math.rad(-15),math.rad(0),math.rad(0)),0.15)
	end
	Humanoid.WalkSpeed = 3
	end
	attack=false
	end
	if k=='e' and attack==false and resting==false and sitting==false and meditating==false and sprint==false then
	sprint=true
	Humanoid.WalkSpeed = 16
	end
end)

mouse.KeyUp:connect(function(k)
if k=='e' and attack==false and resting==false and sitting==false and meditating==false and sprint==true then
sprint=false
Humanoid.WalkSpeed = 3
end
end)

local sine = 0
local change = 1
local val = 0

fat.Event:connect(function()
sine = sine + change
local torvel=(RootPart.Velocity*Vector3.new(1,0,1)).magnitude 
local velderp=RootPart.Velocity.y
hitfloor,posfloor=rayCast(RootPart.Position,(CFrame.new(RootPart.Position,RootPart.Position - Vector3.new(0,1,0))).lookVector,4,Character)
if equipped==true or equipped==false then
if attack==false then
idle=idle+1
else
idle=0
end
if idle>=500 then
if attack==false then
--Sheath()
end
end
if RootPart.Velocity.y > 1 and hitfloor==nil then 
Anim="Jump"
if attack==false then
change=1
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0)*angles(math.rad(-5),math.rad(0),math.rad(0)),0.15)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0)*angles(math.rad(-10),math.rad(0),math.rad(0)),0.15)
RW.C0=clerp(RW.C0,CFrame.new(1.5,0.5,0)*angles(math.rad(-20),math.rad(0),math.rad(20)),0.15)
LW.C0=clerp(LW.C0,CFrame.new(-1.5,0.5,0)*angles(math.rad(-20),math.rad(0),math.rad(-20)),0.15)
RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(-20),math.rad(90),math.rad(0))*angles(math.rad(-10),math.rad(0),math.rad(0)),0.15)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(-20),math.rad(-90),math.rad(0))*angles(math.rad(-10),math.rad(0),math.rad(0)),0.15)
end
elseif RootPart.Velocity.y < -1 and hitfloor==nil then 
Anim="Fall"
if attack==false then
change=1
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,0)*angles(math.rad(5),math.rad(0),math.rad(0)),0.15)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0)*angles(math.rad(10),math.rad(0),math.rad(0)),0.15)
RW.C0=clerp(RW.C0,CFrame.new(1.5,0.5,0)*angles(math.rad(10),math.rad(0),math.rad(10)),0.15)
LW.C0=clerp(LW.C0,CFrame.new(-1.5,0.5,0)*angles(math.rad(10),math.rad(0),math.rad(-10)),0.15)
RH.C0=clerp(RH.C0,cf(1,-1,0)*angles(math.rad(10),math.rad(90),math.rad(0))*angles(math.rad(-5),math.rad(0),math.rad(0)),0.15)
LH.C0=clerp(LH.C0,cf(-1,-1,0)*angles(math.rad(-10),math.rad(-90),math.rad(0))*angles(math.rad(-5),math.rad(0),math.rad(0)),0.15)
end
elseif torvel<1 and hitfloor~=nil then
Anim="Idle"
if attack==false and sitting==false and resting==false and meditating==false then
change=1
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,-0.1+0.1*math.cos(sine/30))*angles(math.rad(-2.5*math.cos(sine/30)),math.rad(0),math.rad(0)),0.15)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0)*angles(math.rad(-5*math.cos(sine/30))+ -math.sin(sine/30)/15,math.rad(0),math.rad(0)),0.15)
RW.C0=clerp(RW.C0,CFrame.new(1.5,0.55+0.05*math.cos(sine/30)+ -math.sin(sine/30)/40,0)*angles(math.rad(5-2.5*math.cos(sine/30)),math.rad(0),math.rad(10+5*math.cos(sine/30))+ math.sin(sine/30)/20),0.15)
LW.C0=clerp(LW.C0,CFrame.new(-1.5,0.55+0.05*math.cos(sine/30)+ -math.sin(sine/30)/40,0)*angles(math.rad(5-2.5*math.cos(sine/30)),math.rad(0),math.rad(-10-5*math.cos(sine/30))+ -math.sin(sine/30)/20),0.15)
RH.C0=clerp(RH.C0,cf(1,-0.9-0.1*math.cos(sine/30),0.025*math.cos(sine/30))*angles(math.rad(-2.5*math.cos(sine/30)),math.rad(90),math.rad(0))*angles(math.rad(-2.5),math.rad(0),math.rad(0)),0.15)
LH.C0=clerp(LH.C0,cf(-1,-0.9-0.1*math.cos(sine/30),0.025*math.cos(sine/30))*angles(math.rad(-2.5*math.cos(sine/30)),math.rad(-90),math.rad(0))*angles(math.rad(-2.5),math.rad(0),math.rad(0)),0.15)
else
if attack==false and sitting==true and resting==false and meditating==false then
change=1
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,-1.9)*angles(math.rad(-45-2.5*math.cos(sine/30)),math.rad(0),math.rad(0)),0.15)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0)*angles(math.rad(-5*math.cos(sine/30))+ -math.sin(sine/30)/15,math.rad(0),math.rad(0)),0.15)
RW.C0=clerp(RW.C0,CFrame.new(1.5,0.45,0)*angles(math.rad(-45-2.5*math.cos(sine/30)),math.rad(0),math.rad(10)),0.15)
LW.C0=clerp(LW.C0,CFrame.new(-1.5,0.45,0)*angles(math.rad(-45-2.5*math.cos(sine/30)),math.rad(0),math.rad(-10)),0.15)
RH.C0=clerp(RH.C0,cf(1,-1,0.025*math.cos(sine/30))*angles(math.rad(45-2.5*math.cos(sine/30)),math.rad(90),math.rad(0))*angles(math.rad(-2.5),math.rad(0),math.rad(0)),0.15)
LH.C0=clerp(LH.C0,cf(-1,-1,0.025*math.cos(sine/30))*angles(math.rad(45-2.5*math.cos(sine/30)),math.rad(-90),math.rad(0))*angles(math.rad(-2.5),math.rad(0),math.rad(0)),0.15)
else
if attack==false and sitting==false and resting==true and meditating==false then
change=1
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,-2.3)*angles(math.rad(-80-1*math.cos(sine/30)),math.rad(0),math.rad(0)),0.15)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0)*angles(math.rad(5-1*math.cos(sine/30))+ -math.sin(sine/30)/15,math.rad(0),math.rad(0)),0.15)
RW.C0=clerp(RW.C0,CFrame.new(1,0.75,0.1)*angles(math.rad(215-1*math.cos(sine/30)),math.rad(0),math.rad(-45)),0.15)
LW.C0=clerp(LW.C0,CFrame.new(-1,0.75,0.1)*angles(math.rad(200-1*math.cos(sine/30)),math.rad(0),math.rad(45)),0.15)
RH.C0=clerp(RH.C0,cf(1,-1,0.025*math.cos(sine/30))*angles(math.rad(25-1*math.cos(sine/30)),math.rad(90),math.rad(0))*angles(math.rad(20),math.rad(0),math.rad(0)),0.15)
LH.C0=clerp(LH.C0,cf(-1,-1,0.025*math.cos(sine/30))*angles(math.rad(10-1*math.cos(sine/30)),math.rad(-90),math.rad(0))*angles(math.rad(20),math.rad(0),math.rad(0)),0.15)
else
if attack==false and sitting==false and resting==false and meditating==true then
change=1
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,-1.9)*angles(math.rad(10-1*math.cos(sine/30)),math.rad(0),math.rad(0)),0.15)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0)*angles(math.rad(10-1*math.cos(sine/30))+ -math.sin(sine/30)/15,math.rad(0),math.rad(0)),0.15)
RW.C0=clerp(RW.C0,CFrame.new(1.25,0.5,-0.5)*angles(math.rad(0),math.rad(165),math.rad(90)),0.15)
LW.C0=clerp(LW.C0,CFrame.new(-1.25,0.5,-0.5)*angles(math.rad(0),math.rad(-165),math.rad(-90)),0.15)
RH.C0=clerp(RH.C0,cf(1,-1,0.1)*angles(math.rad(-30),math.rad(75),math.rad(0))*angles(math.rad(80),math.rad(0),math.rad(0)),0.15)
LH.C0=clerp(LH.C0,cf(-1,-1,0.1)*angles(math.rad(-30),math.rad(-75),math.rad(0))*angles(math.rad(80),math.rad(0),math.rad(0)),0.15)
end
end
end
end
elseif torvel>2 and torvel<22 and hitfloor~=nil then
Anim="Walk"
if attack==false and sprint==false then
change=0.5
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,-0.175+0.025*math.cos(sine/3.5)+ -math.sin(sine/3.5)/7)*angles(math.rad(5-2.5*math.cos(sine/3.5)),math.rad(0),math.rad(10*math.cos(sine/7))),0.15)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0+0.025*math.cos(sine/3.5))*angles(math.rad(0-2.5*math.cos(sine/3.5)),math.rad(1.5*math.cos(sine/7)),math.rad(-7.5*math.cos(sine/7))),0.15)
RW.C0=clerp(RW.C0,CFrame.new(1.5,0.5,0)*angles(math.rad(30*math.cos(sine/7))+ math.sin(sine/7)/2.5,math.rad(0),math.rad(10)),0.15)
LW.C0=clerp(LW.C0,CFrame.new(-1.5,0.5,0)*angles(math.rad(-30*math.cos(sine/7))+ -math.sin(sine/7)/2.5,math.rad(0),math.rad(-10)),0.15)
RH.C0=clerp(RH.C0,cf(1,-0.925-0.5*math.cos(sine/7)/2,0.5*math.cos(sine/7)/2)*angles(math.rad(-15-15*math.cos(sine/7))+ -math.sin(sine/7)/2.5,math.rad(90-10*math.cos(sine/7)),math.rad(0))*angles(math.rad(0+2.5*math.cos(sine/7)),math.rad(0),math.rad(0)),0.15)
LH.C0=clerp(LH.C0,cf(-1,-0.925+0.5*math.cos(sine/7)/2,-0.5*math.cos(sine/7)/2)*angles(math.rad(-15+15*math.cos(sine/7))+ math.sin(sine/7)/2.5,math.rad(-90-10*math.cos(sine/7)),math.rad(0))*angles(math.rad(0-2.5*math.cos(sine/7)),math.rad(0),math.rad(0)),0.15)
elseif attack==false and sprint==true then
change=1
RootJoint.C0=clerp(RootJoint.C0,RootCF*cf(0,0,-0.1+0.1*math.cos(sine/3.5)+ -math.sin(sine/3.5)/7)*angles(math.rad(5-2.5*math.cos(sine/3.5)),math.rad(0),math.rad(10*math.cos(sine/7))),0.15)
Torso.Neck.C0=clerp(Torso.Neck.C0,necko*cf(0,0,0+0.025*math.cos(sine/3.5))*angles(math.rad(0-2.5*math.cos(sine/3.5)),math.rad(1.5*math.cos(sine/7)),math.rad(-7.5*math.cos(sine/7))),0.15)
RW.C0=clerp(RW.C0,CFrame.new(1.5,0.5,0)*angles(math.rad(45*math.cos(sine/7))+ math.sin(sine/7)/2.5,math.rad(0),math.rad(10)),0.15)
LW.C0=clerp(LW.C0,CFrame.new(-1.5,0.5,0)*angles(math.rad(-45*math.cos(sine/7))+ -math.sin(sine/7)/2.5,math.rad(0),math.rad(-10)),0.15)
RH.C0=clerp(RH.C0,cf(1,-0.9-0.25*math.cos(sine/7)/2,0.5*math.cos(sine/7)/2)*angles(math.rad(-15-45*math.cos(sine/7))+ -math.sin(sine/7)/2.5,math.rad(90-10*math.cos(sine/7)),math.rad(0))*angles(math.rad(0+2.5*math.cos(sine/7)),math.rad(0),math.rad(0)),0.15)
LH.C0=clerp(LH.C0,cf(-1,-0.9+0.25*math.cos(sine/7)/2,-0.5*math.cos(sine/7)/2)*angles(math.rad(-15+45*math.cos(sine/7))+ math.sin(sine/7)/2.5,math.rad(-90-10*math.cos(sine/7)),math.rad(0))*angles(math.rad(0-2.5*math.cos(sine/7)),math.rad(0),math.rad(0)),0.15)
end
elseif torvel<22 and hitfloor~=nil then
Anim="Run"
if attack==false then
end
end
end
end)
