import pygame
import random

pygame.init()
pygame.mixer.init()

largura = 800
altura = 600
tela = pygame.display.set_mode((largura, altura))
pygame.display.set_caption("Foguete Blaze")

branco = (255, 255, 255)
preto = (0, 0, 0)
vermelho = (255, 0, 0)


nave_img = pygame.image.load("Foguete Blaze.png")
asteroide_img = pygame.image.load("asteroide_screen.png")
fundo_img = pygame.image.load("Espaçosideral_screen.jpg")
tiro_img = pygame.image.load("MIÍSSIL.gif")
monstro_img = pygame.image.load("MONSTRO.png")  # Imagem do monstro


alexandre_img = pygame.image.load("Alexandre.jpg")
silvia_img = pygame.image.load("Silvia.jpg")
dani_img = pygame.image.load("Dani.jpg")


nave_img = pygame.transform.scale(nave_img, (50, 50))
asteroide_img = pygame.transform.scale(asteroide_img, (50, 50))
fundo_img = pygame.transform.scale(fundo_img, (largura, altura))
tiro_img = pygame.transform.scale(tiro_img, (10, 20))
monstro_img = pygame.transform.scale(monstro_img, (80, 80))  # Escala do monstro


