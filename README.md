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

alexandre_img = pygame.transform.scale(alexandre_img, (80, 80))
silvia_img = pygame.transform.scale(silvia_img, (80, 80))
dani_img = pygame.transform.scale(dani_img, (80, 80))

class Nave(pygame.sprite.Sprite):
    def __init__(self):
        pygame.sprite.Sprite.__init__(self)
        self.image = nave_img
        self.rect = self.image.get_rect()
        self.rect.centerx = largura // 2
        self.rect.bottom = altura - 10
        self.speedx = 0

    def update(self):
        self.speedx = 0
        keystate = pygame.key.get_pressed()
        if keystate[pygame.K_LEFT]:
            self.speedx = -5
        if keystate[pygame.K_RIGHT]:
            self.speedx = 5
        self.rect.x += self.speedx
        if self.rect.left < 0:
            self.rect.left = 0
        if self.rect.right > largura:
            self.rect.right = largura

            class Asteroide(pygame.sprite.Sprite):
    def __init__(self, velocidade_base):
        pygame.sprite.Sprite.__init__(self)
        self.image = asteroide_img
        self.rect = self.image.get_rect()
        self.rect.x = random.randrange(largura - self.rect.width)
        self.rect.y = random.randrange(-100, -40)
        self.speedy = random.randrange(1, 8) * velocidade_base
        self.speedx = random.randrange(-3, 3) * velocidade_base

    def update(self):
        self.rect.x += self.speedx
        self.rect.y += self.speedy
        if (
            self.rect.top > altura + 10
            or self.rect.left < -25
            or self.rect.right > largura + 20
        ):
            self.rect.x = random.randrange(largura - self.rect.width)
            self.rect.y = random.randrange(-100, -40)
            self.speedy = random.randrange(1, 8)
            self.speedx = random.randrange(-3, 3)

            class Projeteil(pygame.sprite.Sprite):
    def __init__(self, nave):
        pygame.sprite.Sprite.__init__(self)
        self.image = tiro_img  # Usa a imagem carregada
        self.rect = self.image.get_rect()
        self.rect.centerx = nave.rect.centerx
        self.rect.top = nave.rect.top
        self.speedy = -10  # Velocidade do projétil

    def update(self):
        self.rect.y += self.speedy
        if self.rect.bottom < 0:
            self.kill()  # Remove o projétil quando ele sai da tela

            class Monstro(pygame.sprite.Sprite):
    def __init__(self, velocidade_base):
        pygame.sprite.Sprite.__init__(self)
        self.image = monstro_img
        self.rect = self.image.get_rect()
        self.rect.x = random.randrange(largura - self.rect.width)
        self.rect.y = random.randrange(-150, -80)
        self.speedy = random.randrange(1, 3) * velocidade_base  # Mais lento que asteroides
        self.speedx = random.choice([-2, 2]) * velocidade_base  # Movimento lateral
        self.health = 3  # Vida do monstro (quantos tiros aguenta)

    def update(self):
        self.rect.x += self.speedx
        self.rect.y += self.speedy

         # Reverter direção ao atingir as bordas
        if self.rect.left < 0 or self.rect.right > largura:
            self.speedx *= -1

        if self.rect.top > altura + 10:
            self.rect.x = random.randrange(largura - self.rect.width)
            self.rect.y = random.randrange(-150, -80)
            self.speedy = random.randrange(1, 3)
            self.speedx = random.choice([-2, 2])
     def tomar_dano(self):
        self.health -= 1
        if self.health <= 0:
            self.kill()  # Remove o monstro se a vida acabar
            return True  # Indica que o monstro foi destruído
        return False  # Indica que o monstro ainda está vivo

class Boss(pygame.sprite.Sprite):
    def __init__(self, img, velocidade_base):
        pygame.sprite.Sprite.__init__(self)
        self.image = img
        self.rect = self.image.get_rect()
        self.rect.x = random.randrange(largura - self.rect.width)
        self.rect.y = random.randrange(-150, -80)
        self.speedy = random.randrange(1, 3) * velocidade_base
        self.speedx = random.choice([-2, 2]) * velocidade_base
        self.health = 5  # Mais vida que o monstro normal

    def update(self):
        self.rect.x += self.speedx
        self.rect.y += self.speedy

        # Reverter direção ao atingir as bordas
        if self.rect.left < 0 or self.rect.right > largura:
            self.speedx *= -1

        if self.rect.top > altura + 10:
            self.rect.x = random.randrange(largura - self.rect.width)
            self.rect.y = random.randrange(-150, -80)
            self.speedy = random.randrange(1, 3)
            self.speedx = random.choice([-2, 2])

    def tomar_dano(self):
        self.health -= 1
        if self.health <= 0:
            self.kill()
            return True
        return False

class Alexandre(Boss):
    def __init__(self, velocidade_base):
        super().__init__(alexandre_img, velocidade_base)
        

class Silvia(Boss):
    def __init__(self, velocidade_base):
        super().__init__(silvia_img, velocidade_base)


class Dani(Boss):
    def __init__(self, velocidade_base):
        super().__init__(dani_img, velocidade_base)


class ContadorAsteroides:
    def __init__(self):
        self.total_destruidos = 0

    def incrementa(self):
        self.total_destruidos += 1

    def resetar(self):
        self.total_destruidos = 0

    def obter_total(self):
        return self.total_destruidos

        def exibir_placar(surf, texto, tamanho, x, y):
    fonte = pygame.font.Font(None, tamanho)
    texto_surface = fonte.render(texto, True, branco)
    texto_rect = texto_surface.get_rect()
    texto_rect.midtop = (x, y)
    surf.blit(texto_surface, texto_rect)


def desenhar_texto(surf, texto, tamanho, cor, x, y):
    fonte = pygame.font.Font(None, tamanho)
    texto_surface = fonte.render(texto, True, cor)
    texto_rect = texto_surface.get_rect()
    texto_rect.center = (x, y)
    surf.blit(texto_surface, texto_rect)


def carregar_recorde():
    try:
        with open("recorde.txt", "r") as f:
            return int(f.read())
    except FileNotFoundError:
        return 0


def salvar_recorde(recorde):
    with open("recorde.txt", "w") as f:
        f.write(str(recorde))

def criar_asteroides(num_asteroides, velocidade_base):
    for i in range(num_asteroides):
        a = Asteroide(velocidade_base)
        todos_sprites.add(a)
        asteroides.add(a)

# Variáveis Globais
clock = pygame.time.Clock()
pontuacao = 0
recorde = carregar_recorde()
game_over = False
running = True
no_menu = True
asteroide_frequencia = 60  # Inicialmente, um asteroide a cada 60 frames
ultimo_asteroide = pygame.time.get_ticks()
max_asteroides = 20
contador_asteroides = ContadorAsteroides()
dificuldade = 1  # Nível de dificuldade padrão
monstros = pygame.sprite.Group()  # Grupo para os monstros
monstro_frequencia = 7000  # Um monstro a cada 7 segundos (em milissegundos)
bosses = pygame.sprite.Group()  # Grupo para os chefes
boss_frequencia = 10000  # Um boss a cada 10 segundos (em milissegundos)
ultimo_boss = pygame.time.get_ticks()








