import pygame
import random

# Initialize the game
pygame.init()

# Set the screen size
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))

# Set title and clock
pygame.display.set_caption("Fight Game")
clock = pygame.time.Clock()

# Define colors
white = (255, 255, 255)
black = (0, 0, 0)

# Player settings
player_width = 40
player_height = 60
player_x = screen_width // 2
player_y = screen_height - player_height
player_speed = 5

# Enemy settings
enemy_width = 40
enemy_height = 60
enemy_speed = 5
enemy_list = []

# Define player and enemy classes
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface([player_width, player_height])
        self.image.fill(white)
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y

    def update(self, keys):
        if keys[pygame.K_LEFT] and self.rect.x > 0:
            self.rect.x -= player_speed
        if keys[pygame.K_RIGHT] and self.rect.x < screen_width - player_width:
            self.rect.x += player_speed

class Enemy(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface([enemy_width, enemy_height])
        self.image.fill(black)
        self.rect = self.image.get_rect()
        self.rect.x = random.randint(0, screen_width - enemy_width)
        self.rect.y = -enemy_height

    def update(self):
        self.rect.y += enemy_speed
        if self.rect.y > screen_height:
            self.rect.y = -enemy_height
            self.rect.x = random.randint(0, screen_width - enemy_width)

# Create sprite groups
player_group = pygame.sprite.Group()
enemy_group = pygame.sprite.Group()

player = Player()
player_group.add(player)

for i in range(10):
    enemy = Enemy()
    enemy_group.add(enemy)

# Main game loop
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()
    player_group.update(keys)
    enemy_group.update()

    screen.fill(black)
    player_group.draw(screen)
    enemy_group.draw(screen)

    pygame.display.flip(2)
    clock.tick(30)

pygame.quit(3)
