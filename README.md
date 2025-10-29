# Algorithme-d-Euclide-du-pcgd-
Algorithme d'Euclide qui calcul le pcgd d'un nombre

#include <stdio.h>

int main() {
    // Déclaration des variables
    int a, b, reste;

    // Lecture des deux entiers
    printf("Entrez deux entiers naturels : ");
    scanf("%d %d", &a, &b);

    // Affichage des valeurs initiales
    printf("Calcul du PGCD de %d et %d\n", a, b);

    // Application de l'algorithme d'Euclide avec boucle tant que
    while (b != 0) {
        reste = a % b; // Calcul du reste de la division de a par b
        printf("%d = %d * (%d) + %d\n", a, b, a / b, reste);
        a = b;         // On remplace a par b
        b = reste;     // On remplace b par le reste
    }

    // Affichage du PGCD
    printf("Le PGCD est %d\n", a);

    return 0;
}
