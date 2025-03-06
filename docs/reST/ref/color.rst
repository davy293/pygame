import pygame

# Inicializa o Pygame
pygame.init()

# Configurações da tela
LARGURA = 800
ALTURA = 400
tela = pygame.display.set_mode((LARGURA, ALTURA))
pygame.display.set_caption("Jogo Estilo Sonic")

# Cores
AZUL = (0, 0, 255)
VERDE = (0, 255, 0)
AMARELO = (255, 255, 0)

# Definições do personagem
player = pygame.Rect(100, 300, 40, 40)
velocidade_x = 5
velocidade_y = 0
gravidade = 1
pulando = False

# Lista de anéis
aneis = [pygame.Rect(200, 250, 20, 20), pygame.Rect(400, 250, 20, 20), pygame.Rect(600, 250, 20, 20)]
pontos = 0

# Loop principal do jogo
rodando = True
while rodando:
    pygame.time.delay(30)  # Controle de FPS

    # Verifica eventos
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            rodando = False

    # Movimentação
    teclas = pygame.key.get_pressed()
    if teclas[pygame.K_LEFT]:
        player.x -= velocidade_x
    if teclas[pygame.K_RIGHT]:
        player.x += velocidade_x
    if teclas[pygame.K_SPACE] and not pulando:
        velocidade_y = -15  # Pulo
        pulando = True

    # Física do pulo
    velocidade_y += gravidade
    player.y += velocidade_y

    # Limite do chão
    if player.y >= 300:
        player.y = 300
        pulando = False

    # Verifica colisão com os anéis
    for anel in aneis[:]:
        if player.colliderect(anel):
            aneis.remove(anel)
            pontos += 1
            print(f"Pontos: {pontos}")

    # Renderiza o jogo
    tela.fill((0, 0, 0))  # Fundo preto
    pygame.draw.rect(tela, VERDE, (0, 340, LARGURA, 60))  # Chão
    pygame.draw.rect(tela, AZUL, player)  # Personagem
    for anel in aneis:
        pygame.draw.ellipse(tela, AMARELO, anel)  # Anéis

    pygame.display.update()

pygame.quit()

