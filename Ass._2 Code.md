\\ Assignment 2 code:

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    int m, n;
    printf("Enter number of rows (m): ");
    scanf("%d", &m);

    printf("Enter number of columns (n): ");
    scanf("%d", &n);

    char **grid = (char **)malloc(m * sizeof(char *));
    for (int i = 0; i < m; i++) {
        grid[i] = (char *)malloc((n + 1) * sizeof(char));
    }

    printf("Enter the grid rows (each row should have %d uppercase letters):\n", n);
    for (int i = 0; i < m; i++) {
        scanf("%s", grid[i]);
        if ((int)strlen(grid[i]) != n) {
            printf("Error: Row %d length is not %d\n", i, n);
            return 1;
        }
    }

    char word[100];
    printf("Enter the target word (uppercase): ");
    scanf("%s", word);

    int word_len = strlen(word);
    int found = 0;

    for (int i = 0; i < m; i++) {
        for (int j = 0; j + word_len - 1 < n; j++) {
            int k;
            for (k = 0; k < word_len; k++) {
                if (grid[i][j + k] != word[k])
                    break;
            }
            if (k == word_len) {
                printf("Start: (%d, %d) End: (%d, %d)\n", i, j, i, j + word_len - 1);
                found++;
            }
        }
    }

    for (int i = 0; i + word_len - 1 < m; i++) {
        for (int j = 0; j < n; j++) {
            int k;
            for (k = 0; k < word_len; k++) {
                if (grid[i + k][j] != word[k])
                    break;
            }
            if (k == word_len) {
                printf("Start: (%d, %d) End: (%d, %d)\n", i, j, i + word_len - 1, j);
                found++;
            }
        }
    }

    if (!found) {
        printf("Word not found\n");
    }

    for (int i = 0; i < m; i++) {
        free(grid[i]);
    }
    free(grid);

    return 0;
}
