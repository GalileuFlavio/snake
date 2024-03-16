# snake
 Jogo clássico da Cobrinha 

import pygame
import random

# Inicialização do Pygame
pygame.init()

# Definição das variáveis globais
largura = 800
altura = 600
tamanho_celula = 20
cor_cobra = (0, 255, 0)
cor_comida = (255, 0, 0)
cor_fundo = (0, 0, 0)
velocidade = 10

# Configuração da tela
tela = pygame.display.set_mode((largura, altura))
pygame.display.set_caption("Jogo da Cobrinha")

# Função para desenhar a cobra
def desenhar_cobra(corpo_cobra):
    for parte in corpo_cobra:
        pygame.draw.rect(tela, cor_cobra, [parte[0], parte[1], tamanho_celula, tamanho_celula])

# Função principal do jogo
def jogo():
    sair = False
    game_over = False
    velocidade_x = 0
    velocidade_y = 0
    posicao_x = largura / 2
    posicao_y = altura / 2
    corpo_cobra = []
    comprimento_cobra = 1
    comida_x = round(random.randrange(0, largura - tamanho_celula) / 10.0) * 10.0
    comida_y = round(random.randrange(0, altura - tamanho_celula) / 10.0) * 10.0

    # Loop principal do jogo
    while not sair:
        while game_over == True:
            # Mensagem de Game Over
            fonte = pygame.font.SysFont(None, 50)
            mensagem = fonte.render("Game Over! Pressione R para jogar novamente ou Q para sair.", True, cor_cobra)
            tela.blit(mensagem, [largura / 6, altura / 3])
            pygame.display.update()

            # Eventos do teclado para Game Over
            for evento in pygame.event.get():
                if evento.type == pygame.KEYDOWN:
                    if evento.key == pygame.K_q:
                        sair = True
                        game_over = False
                    if evento.key == pygame.K_r:
                        jogo()

        # Eventos do teclado
        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                sair = True
            elif evento.type == pygame.KEYDOWN:
                if evento.key == pygame.K_LEFT:
                    velocidade_x = -tamanho_celula
                    velocidade_y = 0
                elif evento.key == pygame.K_RIGHT:
                    velocidade_x = tamanho_celula
                    velocidade_y = 0
                elif evento.key == pygame.K_UP:
                    velocidade_y = -tamanho_celula
                    velocidade_x = 0
                elif evento.key == pygame.K_DOWN:
                    velocidade_y = tamanho_celula
                    velocidade_x = 0

        # Atualização da posição da cobra
        posicao_x += velocidade_x
        posicao_y += velocidade_y

        # Verificação de colisões com as bordas da tela
        if posicao_x >= largura or posicao_x < 0 or posicao_y >= altura or posicao_y < 0:
            game_over = True

        # Atualização da tela de fundo
        tela.fill(cor_fundo)

        # Desenho da comida
        pygame.draw.rect(tela, cor_comida, [comida_x, comida_y, tamanho_celula, tamanho_celula])

        # Atualização da posição da cobra
        corpo_cobra.append([posicao_x, posicao_y])
        if len(corpo_cobra) > comprimento_cobra:
            del corpo_cobra[0]

        # Verificação de colisão com a comida
        if posicao_x == comida_x and posicao_y == comida_y:
            comprimento_cobra += 1
            comida_x = round(random.randrange(0, largura - tamanho_celula) / 10.0) * 10.0
            comida_y = round(random.randrange(0, altura - tamanho_celula) / 10.0) * 10.0

        # Desenho da cobra
        desenhar_cobra(corpo_cobra)

        # Atualização da tela
        pygame.display.update()

        # Atualização da taxa de quadros
        pygame.time.Clock().tick(velocidade)

    # Finalização do Pygame
    pygame.quit()
    quit()

# Início do jogo
jogo()
