# hse21_H3K4me1_G4_seq_Li_K_human

Выполнил: Семенкович Тимофей
Группа: 2-ая
Организм: Челевок
Гистоновая метка: H3K4me1
Тип клеток: GM23338 
Гистоновая метка: ENCFF474FMV https://www.encodeproject.org/files/ENCFF474FMV/  
Гистоновая метка: ENCFF986GPP https://www.encodeproject.org/files/ENCFF986GPP/

Сохраненная сессия в UCSC GenomeBrowser:  https://genome.ucsc.edu/s/Bruh/hse21_H3K4me1_G4_human 

## Конвертируем координаты с версии генома hg38 на hg19

Скачаем файл hg38ToHg19.over.chain.gz
'''
wget https://hgdownload.cse.ucsc.edu/goldenpath/hg38/liftOver/hg38ToHg19.over.chain.gz
'''
Оставляем только первые 5 столбцов
'''
zcat ENCFF474FMV.bed.gz  |  cut -f1-5 > H3K4me1_GM23338.ENCFF474FMV.hg38.bed
zcat ENCFF986GPP.bed.gz  |  cut -f1-5 > H3K4me1_GM23338.ENCFF986GPP.hg38.bed
'''
Теперь конвертируем координаты с помощью liftOver
'''
liftOver   H3K4me1_GM23338.ENCFF474FMV.hg38.bed   hg38ToHg19.over.chain.gz   H3K4me1_GM23338.ENCFF474FMV.hg19.bed   H3K4me1_GM23338.ENCFF474FMV.unmapped.bed
liftOver   H3K4me1_GM23338.ENCFF986GPP.hg38.bed   hg38ToHg19.over.chain.gz   H3K4me1_GM23338.ENCFF986GPP.hg19.bed   H3K4me1_GM23338.ENCFF986GPP.unmapped.bed
'''

## Строим гистограммы распределения длин пик
С помощью этого КОДА получаем следующие гистограммы:
![image](https://user-images.githubusercontent.com/55275328/121436656-ab811c80-c989-11eb-9447-2232885af36b.png)
![image](https://user-images.githubusercontent.com/55275328/121436677-b340c100-c989-11eb-8faa-ad003e13ec2a.png)
Кол-во пиков: 371532 и 1877541 соответственно



Визуализируем в геном браузере с помощью 
track visibility=dense name="ENCFF474FMV"  description="H3K4me1_GM23338_ENCFF474FMV.hg19.filtered.bed"
https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/blob/main/data/H3K4me1_GM23338_ENCFF474FMV.hg19.filtered.bed?raw=true

track visibility=dense name="ENCFF986GPP"  description="H3K4me1_GM23338_ENCFF986GPP.hg19.filtered.bed"
https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/blob/main/data/H3K4me1_GM23338_ENCFF986GPP.hg19.filtered.bed?raw=true

track visibility=dense name="ChIP_merge"  color=50,50,200   description="H3K4me1_GM23338_merge.hg19.bed"
https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/blob/main/data/H3K4me1_GM23338_merge.hg19.bed?raw=true

Как видим все норм ![image](https://user-images.githubusercontent.com/55275328/121358140-0a6b7500-c93b-11eb-9df3-92c3e6653703.png)

Всего генов 6164 из них 4515 уникальны


Потом вот так 
track visibility=dense name="G4_seq"  color=0,200,0  description="G4_seq.bed"
https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/blob/main/data/G4_seq.bed?raw=true

track visibility=dense name="intersect_with_G4"  color=200,0,0  description="H3K4me1_GM23338.intersect_with_G4.bed"
https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/raw/main/data/H3K4me1_GM23338.intersect_with_G4.bed

Все опять норм ![image](https://user-images.githubusercontent.com/55275328/121381903-cf733c80-c94e-11eb-8413-460bd85a266d.png)

![изображение](https://user-images.githubusercontent.com/55275328/121404156-57b00c80-c964-11eb-90ca-e89099e34550.png)


Наиболее значимые: ![image](https://user-images.githubusercontent.com/55275328/121417642-cdbb7000-c972-11eb-8e1a-fd711409f14a.png)



