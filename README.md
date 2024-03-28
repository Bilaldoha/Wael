import pygame
import random

# إعداد المربعات والألوان
WIDTH = 640
HEIGHT = 480
GRID_SIZE = 20
GRID_WIDTH = WIDTH // GRID_SIZE
GRID_HEIGHT = HEIGHT // GRID_SIZE
BLACK = (0, 0, 0)
GREEN = (0, 255, 0)
RED = (255, 0, 0)

# تهيئة النافذة والساعة
pygame.init()
window = pygame.display.set_mode((WIDTH, HEIGHT))
clock = pygame.time.Clock()

# دالة لرسم المربعات
def draw_square(x, y, color):
    pygame.draw.rect(window, color, (x * GRID_SIZE, y * GRID_SIZE, GRID_SIZE, GRID_SIZE))

# دالة لتحديث الشاشة
def update_screen(snake, food):
    window.fill(BLACK)
    for segment in snake:
        draw_square(segment[0], segment[1], GREEN)
    draw_square(food[0], food[1], RED)
    pygame.display.update()

# تهيئة بداية اللعبة
snake = [(GRID_WIDTH // 2, GRID_HEIGHT // 2)]
direction = random.choice([(1, 0), (-1, 0), (0, 1), (0, -1)])
food = (random.randint(0, GRID_WIDTH - 1), random.randint(0, GRID_HEIGHT - 1))
score = 0

# حلقة اللعب الرئيسية
running = True
while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and direction != (1, 0):
        direction = (-1, 0)
    elif keys[pygame.K_RIGHT] and direction != (-1, 0):
        direction = (1, 0)
    elif keys[pygame.K_UP] and direction != (0, 1):
        direction = (0, -1)
    elif keys[pygame.K_DOWN] and direction != (0, -1):
        direction = (0, 1)

    head = (snake[0][0] + direction[0], snake[0][1] + direction[1])
    snake.insert(0, head)

    if head == food:
        score += 1
        food = (random.randint(0, GRID_WIDTH - 1), random.randint(0, GRID_HEIGHT - 1))
    else:
        snake.pop()

    if (
        head[0] < 0
        or head[0] >= GRID_WIDTH
        or head[1] < 0
        or head[1] >= GRID_HEIGHT
        or head in snake[1:]
    ):
        running = False

    update_screen(snake, food)
    clock.tick(10)

# عرض النتيجة النهائية
print("Game over!")
print("Your score:", score)

pygame.quit()
