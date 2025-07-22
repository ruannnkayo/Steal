local menu = {
    opened = false,
    radius = 120,
    x = love.graphics.getWidth()/2,
    y = love.graphics.getHeight()/2,
    angle = 0,
    qui_offset = 0
}

local minatoImg
local btnBgImg
local icons = {}
local buttons = {
    {icon="icon_status.png",   x=0, y=0, w=110, h=60, hovered=false},
    {icon="icon_play.png",     x=0, y=0, w=110, h=60, hovered=false},
    {icon="icon_teleport.png", x=0, y=0, w=200, h=60, hovered=false},
    {icon="icon_esp.png",      x=0, y=0, w=200, h=60, hovered=false},
    {icon="icon_extra.png",    x=0, y=0, w=200, h=60, hovered=false}
}

local showStats = false
local showTeleport = false
local showESP = false
local showExtra = false

function love.load()
    love.window.setMode(0, 0, {fullscreen = true, resizable = true, vsync = true})
    menu.x = love.graphics.getWidth()/2
    menu.y = love.graphics.getHeight()/2
    minatoImg = love.graphics.newImage("minato.png")
    btnBgImg = love.graphics.newImage("btn_bg.png")
    for i, btn in ipairs(buttons) do
        icons[i] = love.graphics.newImage(btn.icon)
    end
    local startX = menu.x - 220
    local startY = menu.y - 140
    buttons[1].x = startX
    buttons[1].y = startY
    buttons[2].x = startX + buttons[1].w + 20
    buttons[2].y = startY
    for i = 3, #buttons do
        buttons[i].x = startX
        buttons[i].y = startY + (i-2)*80
    end
end

function love.update(dt)
    if menu.opened and menu.angle < math.pi * 2 then
        menu.angle = math.min(menu.angle + dt * 4, math.pi * 2)
    elseif not menu.opened and menu.angle > 0 then
        menu.angle = math.max(menu.angle - dt * 4, 0)
    end
    menu.qui_offset = math.sin(love.timer.getTime() * 2) * 42
end

function isInside(x, y, bx, by, bw, bh)
    return x > bx and x < bx + bw and y > by and y < by + bh
end

function love.draw()
    love.graphics.setColor(0.2, 0.6, 1, 0.7)
    love.graphics.arc("fill", menu.x, menu.y, menu.radius, 0, menu.angle)
    love.graphics.setColor(0, 0, 0, 1)
    love.graphics.arc("line", menu.x, menu.y, menu.radius, 0, menu.angle)

    love.graphics.setColor(1, 0.4, 0.2)
    love.graphics.circle("fill", menu.x, menu.y, 55)
    love.graphics.setColor(1, 1, 1)
    if minatoImg then
        love.graphics.draw(minatoImg, menu.x-60, menu.y-60, 0, 120/minatoImg:getWidth(), 120/minatoImg:getHeight())
    end

    if menu.opened and menu.angle == math.pi * 2 then
        for i, btn in ipairs(buttons) do
            -- Fundo do botão
            love.graphics.setColor(btn.hovered and {0.9, 0.8, 0.1} or {1,1,1})
            love.graphics.draw(btnBgImg, btn.x, btn.y, 0, btn.w/btnBgImg:getWidth(), btn.h/btnBgImg:getHeight())
            -- Ícone do botão centralizado
            love.graphics.setColor(1,1,1)
            local icon = icons[i]
            local scale = math.min((btn.h*0.7)/icon:getHeight(), (btn.w*0.7)/icon:getWidth())
            love.graphics.draw(icon, btn.x + btn.w/2 - (icon:getWidth()*scale)/2, btn.y + btn.h/2 - (icon:getHeight()*scale)/2, 0, scale, scale)
        end
    end

    -- Qui/ícone lateral animado
    local qui_x = menu.x + menu.radius + menu.qui_offset
    local qui_y = menu.y
    love.graphics.setColor(1, 1, 1)
    love.graphics.circle("fill", qui_x, qui_y, 40)
    if minatoImg then
        love.graphics.draw(minatoImg, qui_x-28, qui_y-28, 0, 56/minatoImg:getWidth(), 56/minatoImg:getHeight())
    end

    -- Nenhum texto é desenhado em nenhum lugar!
end

function love.touchpressed(id, x, y, dx, dy, pressure)
    local ww, wh = love.graphics.getDimensions()
    local mx, my = x * ww, y * wh
    if (mx-menu.x)^2 + (my-menu.y)^2 < 55^2 then
        menu.opened = not menu.opened
        showStats = false
        showTeleport = false
        showESP = false
        showExtra = false
        return
    end
    if menu.opened and menu.angle == math.pi * 2 then
        for i, btn in ipairs(buttons) do
            if isInside(mx, my, btn.x, btn.y, btn.w, btn.h) then
                showStats = false
                showTeleport = false
                showESP = false
                showExtra = false
                if i == 1 or i == 2 then
                    showStats = true
                elseif i == 3 then
                    showTeleport = true
                elseif i == 4 then
                    showESP = true
                elseif i == 5 then
                    showExtra = true
                end
            end
        end
    end
end 
