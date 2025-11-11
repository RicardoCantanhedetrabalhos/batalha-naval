#include <stdio.h>
#include <stdbool.h>

#define ROWS 10
#define COLS 10
#define SHIP_SIZE 3
#define WATER 0
#define SHIP 3

/* 
 * Verifica se é possível posicionar um navio de tamanho `size`
 * começando em (r, c) com incremento (dr, dc) sem sair do tabuleiro
 * e sem sobrepor outro navio. Se possível, marca as células com o valor SHIP.
 *
 * Parâmetros:
 *  board - matriz do tabuleiro
 *  r, c  - coordenada inicial (linha, coluna) (0-indexed)
 *  dr, dc- direção: por exemplo (0,1)=direita, (1,0)=baixo, (1,1)=diagonal baixo-direita, (-1,1)=diagonal cima-direita
 *  size  - tamanho do navio
 *
 * Retorna: true se posicionou com sucesso; false caso contrário.
 */
bool place_ship(int board[ROWS][COLS], int r, int c, int dr, int dc, int size) {
    // 1) Verificar limites para todas as células que navio ocuparia
    for (int i = 0; i < size; ++i) {
        int rr = r + i * dr;
        int cc = c + i * dc;
        if (rr < 0 || rr >= ROWS || cc < 0 || cc >= COLS) {
            // Fora do tabuleiro
            return false;
        }
        if (board[rr][cc] != WATER) {
            // Já há algo ocupando (sobreposição)
            return false;
        }
    }

    // 2) Marcar o navio no tabuleiro
    for (int i = 0; i < size; ++i) {
        int rr = r + i * dr;
        int cc = c + i * dc;
        board[rr][cc] = SHIP;
    }
    return true;
}

/* Função para mostrar o tabuleiro no console */
void print_board(int board[ROWS][COLS]) {
    printf("Tabuleiro 10x10 (0 = água, 3 = navio):\n\n");
    // Cabeçalho de colunas (opcional, melhora visual)
    printf("   ");
    for (int c = 0; c < COLS; ++c) {
        printf("%2d ", c);
    }
    printf("\n");

    for (int r = 0; r < ROWS; ++r) {
        printf("%2d ", r); // índice da linha
        for (int c = 0; c < COLS; ++c) {
            printf(" %d ", board[r][c]);
        }
        printf("\n");
    }
}

int main(void) {
    // Inicializa tabuleiro com 0 (água)
    int board[ROWS][COLS];
    for (int r = 0; r < ROWS; ++r)
        for (int c = 0; c < COLS; ++c)
            board[r][c] = WATER;

    // --- Definição das posições dos 4 navios (predefinidas no código) ---
    // Regras: tamanho fixo = 3.
    // Dois navios horizontais/verticais, dois navios diagonais.

    // Navio 1: horizontal, início (1,1), direção direita (dr=0, dc=1)
    bool ok;
    ok = place_ship(board, 1, 1, 0, 1, SHIP_SIZE);
    if (!ok) {
        printf("Erro ao posicionar Navio 1 (horizontal) em (1,1).\n");
        return 1;
    }

    // Navio 2: vertical, início (4,7), direção baixo (dr=1, dc=0)
    ok = place_ship(board, 4, 7, 1, 0, SHIP_SIZE);
    if (!ok) {
        printf("Erro ao posicionar Navio 2 (vertical) em (4,7).\n");
        return 1;
    }

    // Navio 3: diagonal baixo-direita, início (6,2), direção (1,1)
    ok = place_ship(board, 6, 2, 1, 1, SHIP_SIZE);
    if (!ok) {
        printf("Erro ao posicionar Navio 3 (diagonal \\) em (6,2).\n");
        return 1;
    }

    // Navio 4: diagonal cima-direita (dr=-1, dc=1), início (9,6) => ocupa (9,6),(8,7),(7,8)
    ok = place_ship(board, 9, 6, -1, 1, SHIP_SIZE);
    if (!ok) {
        printf("Erro ao posicionar Navio 4 (diagonal /) em (9,6).\n");
        return 1;
    }

    // Exibir o tabuleiro final
    print_board(board);

    return 0;
}
