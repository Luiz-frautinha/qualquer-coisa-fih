# qualquer-coisa-fih
import pygame
import random

# Inicialização do Pygame
pygame.init()

# Configurações da tela
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Aventura do Herói")

# Cores
WHITE = (255, 255, 255)
RED = (255, 0, 0)
BLUE = (0, 0, 255)

# Classe do jogador
class Player(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill(BLUE)
        self.rect = self.image.get_rect()
        self.rect.center = (WIDTH // 2, HEIGHT - 60)
        self.speed = 5

    def update(self, keys):
        if keys[pygame.K_LEFT] and self.rect.left > 0:
            self.rect.x -= self.speed
        if keys[pygame.K_RIGHT] and self.rect.right < WIDTH:
            self.rect.x += self.speed

# Classe dos inimigos
class Enemy(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill(RED)
        self.rect = self.image.get_rect()
        self.rect.x = random.randint(0, WIDTH - 50)
        self.rect.y = random.randint(-100, -40)
        self.speed = random.randint(2, 6)

    def update(self):
        self.rect.y += self.speed
        if self.rect.top > HEIGHT:
            self.rect.y = random.randint(-100, -40)
            self.rect.x = random.randint(0, WIDTH - 50)
            self.speed = random.randint(2, 6)

# Inicialização do jogo
player = Player()
enemies = pygame.sprite.Group()
for _ in range(5):
    enemies.add(Enemy())
all_sprites = pygame.sprite.Group(player, *enemies)

# Loop principal do jogo
running = True
clock = pygame.time.Clock()
while running:
    clock.tick(60)
    screen.fill(WHITE)
    
    keys = pygame.key.get_pressed()
    player.update(keys)
    enemies.update()
    
    for entity in all_sprites:
        screen.blit(entity.image, entity.rect)
    
    for enemy in enemies:
        if player.rect.colliderect(enemy.rect):
            print("Game Over!")
            running = False
    
    pygame.display.flip()
    
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

pygame.quit()
