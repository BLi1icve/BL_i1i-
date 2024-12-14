import pygame
import random

# 게임 초기 설정
pygame.init()
screen = pygame.display.set_mode((400, 600))
clock = pygame.time.Clock()

# 색상
WHITE = (255, 255, 255)
RED = (255, 0, 0)
BLUE = (0, 0, 255)

# 우주선 설정
spaceship = pygame.Rect(175, 500, 50, 50)

# 블록 설정
blocks = []
block_speed = 5
block_spawn_time = 30  # 블록 생성 간격

# 점수
score = 0
font = pygame.font.Font(None, 36)

# 게임 루프
running = True
while running:
    screen.fill(WHITE)

    # 이벤트 처리
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # 우주선 움직임
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and spaceship.left > 0:
        spaceship.move_ip(-8, 0)
    if keys[pygame.K_RIGHT] and spaceship.right < 400:
        spaceship.move_ip(8, 0)

    # 블록 생성
    if random.randint(1, block_spawn_time) == 1:
        blocks.append(pygame.Rect(random.randint(0, 350), 0, 50, 50))

    # 블록 이동
    for block in blocks[:]:
        block.move_ip(0, block_speed)
        if block.colliderect(spaceship):
            running = False  # 충돌 시 게임 종료
        if block.top > 600:
            blocks.remove(block)
            score += 1

    # 블록 및 우주선 그리기
    pygame.draw.rect(screen, BLUE, spaceship)
    for block in blocks:
        pygame.draw.rect(screen, RED, block)

    # 점수 표시
    score_text = font.render(f"Score: {score}", True, (0, 0, 0))
    screen.blit(score_text, (10, 10))

    # 화면 업데이트
    pygame.display.flip()
    clock.tick(30)

pygame.quit()
