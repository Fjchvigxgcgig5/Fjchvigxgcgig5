import pygame
import random

# Initialize Pygame
pygame.init()

# Set up the screen
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Simple Battle Royale")

# Colors
WHITE = (255, 255, 255)
RED = (255, 0, 0)
GREEN = (0, 255, 0)

# Player attributes
player_size = 50
player_speed = 5
player_pos = [WIDTH / 2, HEIGHT / 2]

# Enemy attributes
enemy_size = 30
enemy_speed = 3
enemies = []

# Main game loop
running = True
while running:
    screen.fill(WHITE)

    # Handle events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Player movement
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        player_pos[0] -= player_speed
    if keys[pygame.K_RIGHT]:
        player_pos[0] += player_speed
    if keys[pygame.K_UP]:
        player_pos[1] -= player_speed
    if keys[pygame.K_DOWN]:
        player_pos[1] += player_speed

    # Keep player within screen bounds
    player_pos[0] = max(0, min(WIDTH - player_size, player_pos[0]))
    player_pos[1] = max(0, min(HEIGHT - player_size, player_pos[1]))

    # Draw player
    pygame.draw.rect(screen, RED, (player_pos[0], player_pos[1], player_size, player_size))

    # Spawn enemies
    if len(enemies) < 5:
        enemy_pos = [random.randrange(0, WIDTH - enemy_size), random.randrange(0, HEIGHT - enemy_size)]
        enemies.append(enemy_pos)

    # Move and draw enemies
    for enemy_pos in enemies:
        enemy_pos[0] += random.choice([-1, 0, 1]) * enemy_speed
        enemy_pos[1] += random.choice([-1, 0, 1]) * enemy_speed
        pygame.draw.rect(screen, GREEN, (enemy_pos[0], enemy_pos[1], enemy_size, enemy_size))

    # Collision detection with enemies
    for enemy_pos in enemies[:]:
        if player_pos[0] < enemy_pos[0] + enemy_size and \
           player_pos[0] + player_size > enemy_pos[0] and \
           player_pos[1] < enemy_pos[1] + enemy_size and \
           player_pos[1] + player_size > enemy_pos[1]:
            enemies.remove(enemy_pos)
            print("Collision!")

    pygame.display.update()

pygame.quit()
