#include <stdio.h>
#include <stdlib.h>

/* --------- 1) PGCD (algorithme d'Euclide) --------- */
/* Renvoie le PGCD de a et b (valeurs absolues) */
long long pgcd(long long a, long long b) {
    if (a < 0) a = -a;
    if (b < 0) b = -b;
    while (b != 0) {
        long long r = a % b;
        a = b;
        b = r;
    }
    return a;
}

/* --------- 2) Euclide étendu (version simple, récursive) --------- */
/* Calcule d = gcd(a,b) et trouve x,y tels que a*x + b*y = d
   Retourne d et met x_ptr,y_ptr. */
long long euclide_etendu(long long a, long long b, long long *x_ptr, long long *y_ptr) {
    if (b == 0) {
        /* Cas de base : gcd(a,0) = |a|, solution x = sign(a), y = 0 */
        if (a >= 0) {
            *x_ptr = 1;
            *y_ptr = 0;
            return a;
        } else {
            *x_ptr = -1;
            *y_ptr = 0;
            return -a;
        }
    }
    long long x1, y1;
    long long d = euclide_etendu(b, a % b, &x1, &y1);
    /* On a : b*x1 + (a%b)*y1 = d
       Mais a%b = a - (a/b)*b, donc
       a*y1 + b*(x1 - (a/b)*y1) = d
       D'où : x = y1 ; y = x1 - (a/b)*y1
    */
    *x_ptr = y1;
    *y_ptr = x1 - (a / b) * y1;
    return d;
}

int main(void) {
    long long a, b, c;
    printf("Résolution de l'équation diophantienne ax + by = c\n");
    printf("Entrez a b c (ex : 21 14 35) : ");

    if (scanf("%lld %lld %lld", &a, &b, &c) != 3) {
        printf("Entrée invalide. Veuillez saisir trois entiers.\n");
        return 1;
    }

    /* ---------- Tâche 1 : PGCD ---------- */
    long long d = pgcd(a, b);
    printf("\n1) PGCD(|%lld|, |%lld|) = %lld\n", a, b, d);

    /* ---------- Tâche 3 : Vérification d'existence ---------- */
    if (c % d != 0) {
        printf("\n2) Vérification d'existence :\n");
        printf("Pas de solution entière car %lld n'est pas divisible par %lld.\n", c, d);
        return 0;
    } else {
        printf("\n2) Vérification d'existence :\n");
        printf("Il existe au moins une solution entière car %lld est divisible par %lld.\n", c, d);
    }

    /* ---------- Tâche 2 : Euclide étendu pour trouver x0,y0 (pour d) ---------- */
    long long x0, y0;
    long long d_ext = euclide_etendu(a, b, &x0, &y0);
    /* d_ext devrait être égal à d (en valeur absolue) */
    if (d_ext != d && d_ext != -d) {
        /* cas improbable, mais on le signale */
        printf("Attention : Euclide étendu a donné d = %lld tandis que pgcd donne %lld\n", d_ext, d);
    }
    printf("\n3) Euclide étendu (trouve x0,y0 pour d) :\n");
    printf("On obtient d = %lld = (%lld)*(%lld) + (%lld)*(%lld)\n", d, a, x0, b, y0);

    /* ---------- Tâche 4 : Calcul d'une solution particulière pour ax + by = c ---------- */
    long long facteur = c / d; /* entier */
    long long xp = x0 * facteur;
    long long yp = y0 * facteur;
    printf("\n4) Solution particulière pour ax + by = c :\n");
    printf("(xp, yp) = (%lld, %lld)\n", xp, yp);
    printf("Vérification : %lld*(%lld) + %lld*(%lld) = %lld\n", a, xp, b, yp, a*xp + b*yp);

    /* ---------- Tâche 5 : Forme générale des solutions ---------- */
    /* Toutes les solutions : x = xp + (b/d)*t ; y = yp - (a/d)*t , pour t entier */
    long long step_x = b / d; /* coefficient devant t pour x */
    long long step_y = - (a / d); /* coefficient devant t pour y */
    printf("\n5) Forme générale des solutions :\n");
    printf("Pour tout entier t,\n");
    printf("  x = %lld + (%lld) * t\n", xp, step_x);
    printf("  y = %lld + (%lld) * t\n", yp, step_y);
    printf("Ici b/d = %lld et -a/d = %lld\n", step_x, step_y);

    /* ---------- Tâche 6 : Interaction / affichage d'exemples ---------- */
    printf("\n6) Quelques solutions exemples (t = -2..2) :\n");
    for (long long t = -2; t <= 2; ++t) {
        long long x = xp + step_x * t;
        long long y = yp + step_y * t;
        printf(" t = %2lld -> x = %8lld , y = %8lld ; a*x + b*y = %lld\n",
               t, x, y, a*x + b*y);
    }

    /* ---------- Fin / explications courtes ---------- */
    printf("\nBonne fin :\n");
    printf(" - PGCD : algorithme d'Euclide (boucle classique)\n");
    printf(" - Euclide étendu : version récursive simple qui construit x et y\n");
    printf(" - Si d | c alors on multiplie la solution de a*x + b*y = d par c/d\n");
    printf(" - Forme générale : x = xp + (b/d)*t ; y = yp - (a/d)*t\n");

    return 0;
}