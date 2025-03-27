#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define N 10  // Tamanho do tabuleiro
#define HABILIDADE_SIZE 5 // Tamanho das matrizes de habilidade

// Funções para exibição e manipulação do tabuleiro e habilidades
void exibirTabuleiro(int tabuleiro[N][N]);
void inicializarTabuleiro(int tabuleiro[N][N]);
void gerarHabilidade(int habilidade[HABILIDADE_SIZE][HABILIDADE_SIZE], const char tipo);
void sobreporHabilidade(int tabuleiro[N][N], int habilidade[HABILIDADE_SIZE][HABILIDADE_SIZE], int linha, int coluna);
void exibirTutorial(void);
void pausa(void);
void jogarRodada(int tabuleiro[N][N], char* jogador);
void jogarTurno(char* jogador1, char* jogador2);

// Função para exibir coordenadas geográficas
void exibirCoordenadasGeograficas(void) {
    printf("\n== Coordenadas Geográficas do Tabuleiro ==\n");
    printf("Tabuleiro (A-J para colunas, 1-10 para linhas):\n");
    printf("  A B C D E F G H I J\n");
    for (int i = 0; i < N; i++) {
        printf("%2d ", i + 1);  // Usando 2 dígitos para as linhas
        for (int j = 0; j < N; j++) {
            printf("~ ");  // Representação genérica da água
        }
        printf("\n");
    }
    printf("\nA = 0, B = 1, C = 2, ..., J = 9 (colunas)\n1 = Linha 1, 2 = Linha 2, ..., 10 = Linha 10\n");
    pausa();
}

int main() {
    char jogador1[30], jogador2[30];
    int tabuleiro1[N][N];
    int habilidadeCone[HABILIDADE_SIZE][HABILIDADE_SIZE];
    int habilidadeCruz[HABILIDADE_SIZE][HABILIDADE_SIZE];
    int habilidadeOctaedro[HABILIDADE_SIZE][HABILIDADE_SIZE];

    // Exibir tutorial
    exibirTutorial();
    pausa();
    system("clear");  // Se estiver no Windows, use system("cls")

    // Solicitar nome dos jogadores
    printf("Digite o nome do jogador 1: ");
    fgets(jogador1, sizeof(jogador1), stdin);
    printf("Digite o nome do jogador 2: ");
    fgets(jogador2, sizeof(jogador2), stdin);

    // Inicializar o tabuleiro com água e colocar alguns navios aleatórios
    inicializarTabuleiro(tabuleiro1);

    // Gerar as matrizes de habilidade
    gerarHabilidade(habilidadeCone, 'C');
    gerarHabilidade(habilidadeCruz, 'X');
    gerarHabilidade(habilidadeOctaedro, 'O');

    // Exibir as coordenadas geográficas do tabuleiro
    exibirCoordenadasGeograficas();

    // Jogar o turno com os dois jogadores
    jogarTurno(jogador1, jogador2);

    return 0;
}

// Função para exibir o tabuleiro
void exibirTabuleiro(int tabuleiro[N][N]) {
    printf("\nTabuleiro Atual:\n");
    printf("   A B C D E F G H I J\n");  // Afastando os números
    for (int i = 0; i < N; i++) {
        printf("%2d ", i + 1);  // Exibe a linha do tabuleiro com dois dígitos para alinhamento
        for (int j = 0; j < N; j++) {
            if (tabuleiro[i][j] == 0)
                printf("~ ");  // Representa a água
            else if (tabuleiro[i][j] == 3)
                printf("N ");  // Representa o navio
            else if (tabuleiro[i][j] == 5)
                printf("X ");  // Representa a área afetada pela habilidade
        }
        printf("\n");
    }
}

// Função para inicializar o tabuleiro com água e navios aleatórios
void inicializarTabuleiro(int tabuleiro[N][N]) {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            tabuleiro[i][j] = 0;  // Inicializa com água
        }
    }

    // Colocar navios aleatórios no tabuleiro
    srand(time(NULL));
    for (int i = 0; i < 10; i++) {
        int linha = rand() % N;
        int coluna = rand() % N;
        tabuleiro[linha][coluna] = 3;  // Navio
    }
}

// Função para gerar as matrizes de habilidade
void gerarHabilidade(int habilidade[HABILIDADE_SIZE][HABILIDADE_SIZE], const char tipo) {
    // Inicializar matriz com zeros
    for (int i = 0; i < HABILIDADE_SIZE; i++) {
        for (int j = 0; j < HABILIDADE_SIZE; j++) {
            habilidade[i][j] = 0;
        }
    }

    if (tipo == 'C') {  // Cone
        for (int i = 0; i < HABILIDADE_SIZE; i++) {
            for (int j = HABILIDADE_SIZE / 2 - i; j <= HABILIDADE_SIZE / 2 + i; j++) {
                if (j >= 0 && j < HABILIDADE_SIZE)
                    habilidade[i][j] = 1;
            }
        }
    } else if (tipo == 'X') {  // Cruz
        for (int i = 0; i < HABILIDADE_SIZE; i++) {
            habilidade[HABILIDADE_SIZE / 2][i] = 1;  // Linha central
            habilidade[i][HABILIDADE_SIZE / 2] = 1;  // Coluna central
        }
    } else if (tipo == 'O') {  // Octaedro
        for (int i = 0; i < HABILIDADE_SIZE; i++) {
            for (int j = HABILIDADE_SIZE / 2 - i; j <= HABILIDADE_SIZE / 2 + i; j++) {
                if (j >= 0 && j < HABILIDADE_SIZE)
                    habilidade[i][j] = 1;
            }
        }
        for (int i = HABILIDADE_SIZE / 2 + 1; i < HABILIDADE_SIZE; i++) {
            for (int j = i - HABILIDADE_SIZE / 2; j < HABILIDADE_SIZE - (i - HABILIDADE_SIZE / 2); j++) {
                habilidade[i][j] = 1;
            }
        }
    }
}

// Função para sobrepor a habilidade no tabuleiro
void sobreporHabilidade(int tabuleiro[N][N], int habilidade[HABILIDADE_SIZE][HABILIDADE_SIZE], int linha, int coluna) {
    for (int i = 0; i < HABILIDADE_SIZE; i++) {
        for (int j = 0; j < HABILIDADE_SIZE; j++) {
            int x = linha + i - HABILIDADE_SIZE / 2;
            int y = coluna + j - HABILIDADE_SIZE / 2;

            if (x >= 0 && x < N && y >= 0 && y < N && habilidade[i][j] == 1) {
                tabuleiro[x][y] = 5;  // Marca as posições afetadas
            }
        }
    }
}

// Função para exibir o tutorial do jogo
void exibirTutorial(void) {
    printf("==== Tutorial Batalha Naval ====\n\n");
    printf("Neste jogo, você jogará contra o computador ou outro jogador, que terá um tabuleiro com navios escondidos.\n");
    printf("Você poderá usar três habilidades especiais: Cone, Cruz e Octaedro, que afetam áreas do tabuleiro.\n");
    printf("As habilidades têm as seguintes formas:\n");
    printf("- Cone: Área expandida de forma triangular.\n");
    printf("- Cruz: Área afetada em formato de cruz, com centro no ponto de origem.\n");
    printf("- Octaedro: Área afetada em formato de losango, com centro no ponto de origem.\n\n");
    printf("Objetivo: Aplicar as habilidades para atingir os navios do oponente!\n");
    printf("Boa sorte e divirta-se!\n\n");
    printf("Cada coordenada no tabuleiro é composta por uma linha (1-10) e uma coluna (A-J).\n");
    printf("Exemplo: A1 é a célula da primeira linha e primeira coluna.\n");
}

// Função para pausar a execução até pressionar Enter
void pausa(void) {
    printf("\nPressione Enter para continuar...");
    while(getchar() != '\n');
}

// Função para jogar uma rodada do jogo
void jogarRodada(int tabuleiro[N][N], char* jogador) {
    int habilidadeEscolhida;
    int linha, coluna;
    printf("\nTurno de %s", jogador);
    exibirTabuleiro(tabuleiro);

    printf("\nEscolha uma habilidade (1 - Cone, 2 - Cruz, 3 - Octaedro): ");
    scanf("%d", &habilidadeEscolhida);

    printf("Escolha a linha e coluna para aplicar a habilidade (0-9): ");
    scanf("%d-%d", &linha, &coluna);

    if (habilidadeEscolhida == 1) {
        int habilidade[HABILIDADE_SIZE][HABILIDADE_SIZE];
        gerarHabilidade(habilidade, 'C');
        sobreporHabilidade(tabuleiro, habilidade, linha, coluna);
    } else if (habilidadeEscolhida == 2) {
        int habilidade[HABILIDADE_SIZE][HABILIDADE_SIZE];
        gerarHabilidade(habilidade, 'X');
        sobreporHabilidade(tabuleiro, habilidade, linha, coluna);
    } else if (habilidadeEscolhida == 3) {
        int habilidade[HABILIDADE_SIZE][HABILIDADE_SIZE];
        gerarHabilidade(habilidade, 'O');
        sobreporHabilidade(tabuleiro, habilidade, linha, coluna);
    }

    printf("Turno de %s terminou.\n", jogador);
    pausa();
}

// Função para jogar os turnos alternados entre os jogadores
void jogarTurno(char* jogador1, char* jogador2) {
    int tabuleiro1[N][N];
    inicializarTabuleiro(tabuleiro1);

    while (1) {
        jogarRodada(tabuleiro1, jogador1);
        system("clear");
        jogarRodada(tabuleiro1, jogador2);
        system("clear");
    }
}
