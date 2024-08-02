from pygame import *
from random import randint

# Initialize Pygame
init()

# Add root dir

# Background music
mixer.init()
mixer.music.load('OpeningBoboiboy.mp3')
mixer.music.play()
fire_sound = mixer.Sound('fire.mp3')

# Fonts and labels
font.init()
font1 = font.Font(None, 80)
win = font1.render('YOU WIN!', True, (255, 255, 255))
lose = font1.render('YOU LOSE!', True, (180, 0, 0))
font2 = font.Font(None, 36)

# Image paths
img_back = ("tokoaba.png")
img_hero = ("Boboiboy.png")
img_bullet = ("laser.png")
img_enemy = ("Adu_Du.png")

score = 0  # ships hit
lost = 0
max_lost = 100
lost_minus = 1


# Parent class for other sprites
class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, size_x, size_y, player_speed):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (size_x, size_y))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y

    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

# Main player class
class Player(GameSprite):
    def update(self):
        keys = key.get_pressed()
        if keys[K_a] and self.rect.x > 5:
            self.rect.x -= self.speed
        if keys[K_d] and self.rect.x < win_width - 80:
            self.rect.x += self.speed

    def fire(self):
        bullet = Bullet(img_bullet, self.rect.centerx, self.rect.top, 30, 55, -10)
        bullets.add(bullet)

# Enemy sprite class
class Enemy(GameSprite):
    def update(self):
        self.rect.y += self.speed
        global lost
        if self.rect.y > win_height:
            self.rect.x = randint(80, win_width - 80)
            self.rect.y = 0
            lost += lost_minus

# Bullet sprite class
class Bullet(GameSprite):
    def update(self):
        self.rect.y += self.speed
        if self.rect.y < 0:
            self.kill()

# Create the window
win_width = 700
win_height = 500
display.set_caption("Shooter")
window = display.set_mode((win_width, win_height))
background = transform.scale(image.load(img_back), (win_width, win_height))

# Create sprites
ship = Player(img_hero, 5, win_height - 100, 80, 100, 12)
monsters = sprite.Group()
for i in range(1,5):
    monster = Enemy(img_enemy, randint(80, win_width - 80), randint(0, 80), 80, 50, randint(1,2))
    monsters.add(monster)

bullets = sprite.Group()

# The "game over" variable: as soon as it is True, the sprites stop working in the main loop
finish = False
run = True  # the flag is cleared with the close window button

# Create a clock object to control the frame rate
clock = time.Clock()

# Main game loop
while run:
    for e in event.get():
        if e.type == QUIT:
            run = False
        elif e.type == KEYDOWN:
            if e.key == K_SPACE:
                fire_sound.play()
                ship.fire()

        collides = sprite.groupcollide(monsters, bullets, True, True)
        for c in collides:
            score += 1
            monster = Enemy(img_enemy, randint(80, win_width - 80), 0, 80, 50, randint(1, 2))
            monsters.add(monster)

        # Check win/lose conditions
        if score == 150  :
            finish = True
            window.blit(win, (200, 200))
            
            
        if lost >= max_lost:
            finish = True
            window.blit(lose, (200, 200))

        if lost >= 5 :
            lost_minus = 2
            
    if not finish:
        window.blit(background, (0, 0))

        # Display scores
        text = font2.render("Score: " + str(score), 1, (255, 255, 255))
        window.blit(text, (10, 20))

        text_lose = font2.render("Missed: " + str(lost), 1, (255, 255, 255))
        window.blit(text_lose, (10, 50))

        # Update and draw sprites
        ship.update()
        monsters.update()
        bullets.update()

        ship.reset()
        monsters.draw(window)
        bullets.draw(window)

        display.update()
    else:
        window.blit(win if score >= 150 else lose, (200, 200))
        display.update()
        time.delay(3000)
        run = False
        
    # Control the frame rate
    clock.tick(60)

quit()
