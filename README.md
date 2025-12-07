# Vigenere-Sifresi/*
Proje: Kriptografi Yardımcısı (Vigenere Şifresi)
*/

#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Sayıyı Harfe Çevirir
char sayi_donusumu(int indeks) {
    return (char)(indeks + 65);
}

// Harfi Sayıya Çevirir
int harf_donusumu(char harf) {
    return toupper(harf) - 65;
}

// ŞİFRELEME FONKSİYONU
// Sonucu 'cikti' dizisine kaydeder.
void vigenere_sifrele(char* metin, char* anahtar, char* cikti) {
    int metin_uzunluk = strlen(metin);
    int anahtar_uzunluk = strlen(anahtar);
    int anahtar_index = 0;
    int j = 0; // Cıktı dizisinin sayacı
    int i; // Döngü değişkeni

    for (i = 0; i < metin_uzunluk; i++) {
        if (isalpha(metin[i])) {
            int P = harf_donusumu(metin[i]);
            int K = harf_donusumu(anahtar[anahtar_index % anahtar_uzunluk]);
            
            // Şifreleme Formülü: (P + K) % 26
            int sifreli_sayi = (P + K) % 26;
            
            cikti[j] = sayi_donusumu(sifreli_sayi);
            
            j++;
            anahtar_index++;
        } else {
            // Harf değilse olduğu gibi ekle (Boşluk vs.)
            cikti[j] = metin[i];
            j++;
        }
    }
    // String'in bittiğini belirtmek için sonuna NULL karakteri ekle
    cikti[j] = '\0';
}

// ŞİFRE ÇÖZME FONKSİYONU 
// Şifreli metni alır, orijinal haline çevirip 'cikti' dizisine yazar.
void vigenere_coz(char* sifreli_metin, char* anahtar, char* cikti) {
    int metin_uzunluk = strlen(sifreli_metin);
    int anahtar_uzunluk = strlen(anahtar);
    int anahtar_index = 0;
    int j = 0;
    int i;

    for (i = 0; i < metin_uzunluk; i++) {
        if (isalpha(sifreli_metin[i])) {
            int C = harf_donusumu(sifreli_metin[i]);
            int K = harf_donusumu(anahtar[anahtar_index % anahtar_uzunluk]);
            
            // Çözme Formülü: (C - K + 26) % 26
            // +26 eklememizin sebebi, çıkarma işlemi negatif çıkarsa mod alırken hata olmamasıdır.
            int cozulmus_sayi = (C - K + 26) % 26;
            
            cikti[j] = sayi_donusumu(cozulmus_sayi);
            
            j++;
            anahtar_index++;
        } else {
            cikti[j] = sifreli_metin[i];
            j++;
        }
    }
    cikti[j] = '\0';
}

int main() {
    char metin[100];
    char anahtar[90];
    
    // Sonuçları tutmak için boş diziler oluşturuyoruz
    char sifreli_sonuc[100]; 
    char cozulmus_sonuc[100];

    printf("--- Vigenere Sifreleme ve Cozme Araci ---\n");
    
    // 1. Veri Alma
    printf("Metni girin (Turkce karakter kullanmayin): ");
    fgets(metin, sizeof(metin), stdin); 

    printf("Anahtar kelimeyi girin: ");
    scanf("%s", anahtar);

    // 2. Şifreleme İşlemi
    vigenere_sifrele(metin, anahtar, sifreli_sonuc);
    printf("\n Sifrelenmis Metin: %s\n", sifreli_sonuc);

    // 3. Şifre Çözme İşlemi (Sağlama Yapma)
    vigenere_coz(sifreli_sonuc, anahtar, cozulmus_sonuc);
    printf(" Sifresi Cozulmus Metin: %s\n", cozulmus_sonuc);

    return 0;
}
