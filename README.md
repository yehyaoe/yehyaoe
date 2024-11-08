import pygame
import random
import time

# Initialize pygame
pygame.init()

# Set up display
WIDTH, HEIGHT = 600, 400
win = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Snake Game")

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)

# Game variables
snake_block = 10
snake_speed = 15
clock = pygame.time.Clock()
font_style = pygame.font.SysFont(None, 30)

def message(msg, color, x, y):
    mesg = font_style.render(msg, True, color)
    win.blit(mesg, [x, y])

def gameLoop():
    game_over = False
    game_close = False

    x1 = WIDTH // 2
    y1 = HEIGHT // 2

    x1_change = 0
    y1_change = 0

    snake_list = []
    length_of_snake = 1

    # Place the food randomly
    foodx = round(random.randrange(0, WIDTH - snake_block) / 10.0) * 10.0
    foody = round(random.randrange(0, HEIGHT - snake_block) / 10.0) * 10.0

    while not game_over:

        while game_close:
            win.fill(WHITE)
            message("Game Over! Press Q-Quit or C-Play Again", RED, WIDTH // 6, HEIGHT // 3)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        gameLoop()

        # Event handling for movement
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -snake_block
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = snake_block
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -snake_block
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = snake_block
                    x1_change = 0

        # Game boundaries
        if x1 >= WIDTH or x1 < 0 or y1 >= HEIGHT or y1 < 0:
            game_close = True

        x1 += x1_change
        y1 += y1_change
        win.fill(BLACK)

        # Draw food
        pygame.draw.rect(win, GREEN, [foodx, foody, snake_block, snake_block])

        # Snake movement
        snake_head = []
        snake_head.append(x1)
        snake_head.append(y1)
        snake_list.append(snake_head)
        if len(snake_list) > length_of_snake:
            del snake_list[0]

        for x in snake_list[:-1]:
            if x == snake_head:
                game_close = True

        for x in snake_list:
            pygame.draw.rect(win, WHITE, [x[0], x[1], snake_block, snake_block])

        # Score display
        message(f"Score: {length_of_snake - 1}", RED, 10, 10)
        pygame.display.update()

        # Check if snake has eaten the food
        if x1 == foodx and y1 == foody:
            foodx = round(random.randrange(0, WIDTH - snake_block) / 10.0) * 10.0
            foody = round(random.randrange(0, HEIGHT - snake_block) / 10.0) * 10.0
            length_of_snake += 1

        clock.tick(snake_speed)

    pygame.quit()
    quit()

# Start the game
gameLoop()
