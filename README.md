# lab_13
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define MAX_SENTENCES 50
#define MAX_LEN 500

void to_lower_ascii(char *s) {
    for (int i = 0; s[i]; i++) {
        if ((unsigned char)s[i] < 128) {
            s[i] = tolower(s[i]);
        }
    }
}

int is_letter(unsigned char c) {
    return c >= 128 || isalpha(c);
}

int contains_word(const char *sentence, const char *word) {
    char s[MAX_LEN], w[MAX_LEN];
    strcpy(s, sentence);
    strcpy(w, word);

    to_lower_ascii(s);
    to_lower_ascii(w);

    int slen = strlen(s);
    int wlen = strlen(w);

    for (int i = 0; i <= slen - wlen; i++) {
        if (memcmp(&s[i], w, wlen) == 0) {
            unsigned char before = (i == 0) ? ' ' : s[i - 1];
            unsigned char after  = s[i + wlen];

            if (!is_letter(before) && !is_letter(after))
                return 1;
        }
    }
    return 0;
}

int main() {
    int n;
    printf("Введіть кількість речень: ");
    scanf("%d", &n);
    getchar();

    char sentences[MAX_SENTENCES][MAX_LEN];

    printf("Введіть речення:\n");
    for (int i = 0; i < n; i++) {
        fgets(sentences[i], MAX_LEN, stdin);
    }

    char keyword[MAX_LEN];
    printf("Введіть ключове слово: ");
    fgets(keyword, MAX_LEN, stdin);
    keyword[strcspn(keyword, "\n")] = 0;

    int found = 0;

    printf("\nРечення, що містять ключове слово:\n");
    for (int i = 0; i < n; i++) {
        if (contains_word(sentences[i], keyword)) {
            printf("- %s", sentences[i]);
            found = 1;
        }
    }

    if (!found) {
        printf("Немає речень, що містять це ключове слово.\n");
    }

    return 0;
}

