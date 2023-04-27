import pygame
import random

# Initialize Pygame
pygame.init()

# Define screen dimensions
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600

# Define colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Create game window
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Hurdle Jumping Game")

# Define font
font = pygame.font.Font(None, 48)

# Define player dimensions and position
player_width = 50
player_height = 100
player_x = SCREEN_WIDTH // 2 - player_width // 2
player_y = SCREEN_HEIGHT - player_height

# Define hurdle dimensions and position
hurdle_width = 50
hurdle_height = 100
hurdle_x = SCREEN_WIDTH
hurdle_y = SCREEN_HEIGHT - hurdle_height

# Define hurdle speed
hurdle_speed = 5

# Define score
score = 0

# Define game over flag
game_over = False

# Game loop
while not game_over:
    # Handle events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            game_over = True
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE and player_y == SCREEN_HEIGHT - player_height:
                # Jump
                player_y -= 100

    # Move player
    if player_y < SCREEN_HEIGHT - player_height:
        player_y += 5

    # Move hurdle
    hurdle_x -= hurdle_speed
    if hurdle_x < 0:
        hurdle_x = SCREEN_WIDTH
        hurdle_y = SCREEN_HEIGHT - hurdle_height - random.randint(0, 200)
        score += 1

    # Check for collision
    if player_x + player_width > hurdle_x and \
       player_x < hurdle_x + hurdle_width and \
       player_y + player_height > hurdle_y:
        game_over = True

    # Clear screen
    screen.fill(WHITE)

    # Draw player
    pygame.draw.rect(screen, BLACK, (player_x, player_y, player_width, player_height))

    # Draw hurdle
    pygame.draw.rect(screen, BLACK, (hurdle_x, hurdle_y, hurdle_width, hurdle_height))

    # Draw score
    score_text = font.render(f"Score: {score}", True, BLACK)
    screen.blit(score_text, (10, 10))

    # Update screen
    pygame.display.flip()

# Quit Pygame
pygame.quit()
