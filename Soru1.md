
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define DIZI_BOYUTU 500

// Fonksiyon prototipleri
void rastgeleDiziOlustur(int dizi[], int boyut);
void diziKopyala(int kaynak[], int hedef[], int boyut);
void birlestirmeliSiralama(int dizi[], int boyut);
void secmeliSiralama(int dizi[], int boyut);
void diziYazdir(int dizi[], int boyut);
double siralamaSuresiOlc(void (*siralamaFonksiyonu)(int[], int), int dizi[], int boyut);

int main() {
    int orijinalDizi[DIZI_BOYUTU];
    int siralanacakDizi[DIZI_BOYUTU];

    // Rastgele sayı üreteciyi başlat
    srand(time(NULL));

    // Rastgele bir dizi oluştur
    rastgeleDiziOlustur(orijinalDizi, DIZI_BOYUTU);

    // Birleştirmeli Sıralama için diziyi kopyala
    diziKopyala(orijinalDizi, siralanacakDizi, DIZI_BOYUTU);
    double birlestirmeSuresi = siralamaSuresiOlc(birlestirmeliSiralama, siralanacakDizi, DIZI_BOYUTU);
    printf("Birlestirmeli Siralama Suresi: %.6f saniye\n", birlestirmeSuresi);

    // Seçmeli Sıralama için diziyi kopyala
    diziKopyala(orijinalDizi, siralanacakDizi, DIZI_BOYUTU);
    double secmeSuresi = siralamaSuresiOlc(secmeliSiralama, siralanacakDizi, DIZI_BOYUTU);
    printf("Secmeli Sıralama Suresi: %.6f saniye\n", secmeSuresi);

    return 0;
}

// Rastgele bir dizi oluşturur
void rastgeleDiziOlustur(int dizi[], int boyut) {
    for (int i = 0; i < boyut; i++) {
        dizi[i] = rand() % 1001; // 0-1000 arasında rastgele bir sayı
    }
}

// Bir diziyi başka bir diziye kopyalar
void diziKopyala(int kaynak[], int hedef[], int boyut) {
    for (int i = 0; i < boyut; i++) {
        hedef[i] = kaynak[i];
    }
}

// Birleştirmeli Sıralama algoritması
void birlestirmeliSiralama(int dizi[], int boyut) {
    for (int i = 1; i < boyut; i++) {
        int anahtar = dizi[i];
        int j = i - 1;
        while (j >= 0 && dizi[j] > anahtar) {
            dizi[j + 1] = dizi[j];
            j--;
        }
        dizi[j + 1] = anahtar;
    }
}

// Seçmeli Sıralama algoritması
void secmeliSiralama(int dizi[], int boyut) {
    for (int i = 0; i < boyut - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < boyut; j++) {
            if (dizi[j] < dizi[minIndex]) {
                minIndex = j;
            }
        }
        int temp = dizi[minIndex];
        dizi[minIndex] = dizi[i];
        dizi[i] = temp;
    }
}

// Sıralama algoritmasının çalışma süresini ölçer
double siralamaSuresiOlc(void (*siralamaFonksiyonu)(int[], int), int dizi[], int boyut) {
    clock_t baslangic, bitis;
    baslangic = clock();
    siralamaFonksiyonu(dizi, boyut);
    bitis = clock();
    return ((double)(bitis - baslangic)) / CLOCKS_PER_SEC;
}
