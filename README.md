# hse21_H3K4me1_G4_seq_Li_K_human

Выполнил: Семенкович Тимофей\
Группа: 2-ая\
Организм: Человек\
Гистоновая метка: H3K4me1\
Тип клеток: GM23338\
Гистоновая метка: ENCFF474FMV https://www.encodeproject.org/files/ENCFF474FMV/  
Гистоновая метка: ENCFF986GPP https://www.encodeproject.org/files/ENCFF986GPP/

Сохраненная сессия в UCSC GenomeBrowser:  https://genome.ucsc.edu/s/Bruh/hse21_H3K4me1_G4_human 

## Конвертируем координаты с версии генома hg38 на hg19

Скачаем файл hg38ToHg19.over.chain.gz
```
wget https://hgdownload.cse.ucsc.edu/goldenpath/hg38/liftOver/hg38ToHg19.over.chain.gz
```
Оставляем только первые 5 столбцов
```
zcat ENCFF474FMV.bed.gz  |  cut -f1-5 > H3K4me1_GM23338.ENCFF474FMV.hg38.bed
zcat ENCFF986GPP.bed.gz  |  cut -f1-5 > H3K4me1_GM23338.ENCFF986GPP.hg38.bed
```
Теперь конвертируем координаты с помощью liftOver
```
liftOver   H3K4me1_GM23338.ENCFF474FMV.hg38.bed   hg38ToHg19.over.chain.gz   H3K4me1_GM23338.ENCFF474FMV.hg19.bed   H3K4me1_GM23338.ENCFF474FMV.unmapped.bed
liftOver   H3K4me1_GM23338.ENCFF986GPP.hg38.bed   hg38ToHg19.over.chain.gz   H3K4me1_GM23338.ENCFF986GPP.hg19.bed   H3K4me1_GM23338.ENCFF986GPP.unmapped.bed
```

## Строим гистограммы распределения длин пик
С помощью [этого](https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/raw/main/src/length_hist.R) кода получаем следующие гистограммы:
![image](https://user-images.githubusercontent.com/55275328/121436656-ab811c80-c989-11eb-9447-2232885af36b.png)  
![image](https://user-images.githubusercontent.com/55275328/121436677-b340c100-c989-11eb-8faa-ad003e13ec2a.png)  
Кол-во пиков: 371532 и 187541 соответственно

## В полученных файлах отрезаем все пики длиннее 5000 оснований
```
awk '$3 - $2 < 5001' ../data/H3K4me1_GM23338_ENCFF474FMV.hg19.bed > ../data/H3K4me1_GM23338_ENCFF474FMV.hg19.filtered.bed
awk '$3 - $2 < 5001' ../data/H3K4me1_GM23338_ENCFF986GPP.hg19.bed > ../data/H3K4me1_GM23338_ENCFF986GPP.hg19.filtered.bed
```
Теперь гистограммы примут следующий вид:
![image](https://user-images.githubusercontent.com/55275328/121437232-a1135280-c98a-11eb-8385-dda4f4855502.png)  
![image](https://user-images.githubusercontent.com/55275328/121437262-aa9cba80-c98a-11eb-9e98-2f64fda19610.png)  
Кол-во пиков: 371034 и 187145 соответственно (уменьшилось на 498 и 396)

## Строим pie-чарты фильтрованных наборов с помощью ChIPseeker
Данные графики получены с помощью этого [кода](https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/raw/main/src/chip_seeker.R)  
ENCFF474FMV:  
![image](https://user-images.githubusercontent.com/55275328/121437564-39a9d280-c98b-11eb-82c7-7f7017033f6c.png)  
ENCFF986GPP:  
![image](https://user-images.githubusercontent.com/55275328/121437579-40d0e080-c98b-11eb-8c1e-5d288db5af83.png)  

## Объединяем два набора отфильтрованных ChIP-seq пиков
Сделаем это с помощью утилиты bedtools merge
```
cat  *.filtered.bed  |   sort -k1,1 -k2,2n   |   bedtools merge   >  H3K4me1_GM23338.merge.hg19.bed
```

## Проверяем объединение в геномном браузере
Визуализируем в геном браузере с помощью 
```
track visibility=dense name="ENCFF474FMV"  description="H3K4me1_GM23338_ENCFF474FMV.hg19.filtered.bed"
https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/blob/main/data/H3K4me1_GM23338_ENCFF474FMV.hg19.filtered.bed?raw=true

track visibility=dense name="ENCFF986GPP"  description="H3K4me1_GM23338_ENCFF986GPP.hg19.filtered.bed"
https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/blob/main/data/H3K4me1_GM23338_ENCFF986GPP.hg19.filtered.bed?raw=true

track visibility=dense name="ChIP_merge"  color=50,50,200   description="H3K4me1_GM23338_merge.hg19.bed"
https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/blob/main/data/H3K4me1_GM23338_merge.hg19.bed?raw=true
```
Как видим все в порядке ![image](https://user-images.githubusercontent.com/55275328/121358140-0a6b7500-c93b-11eb-9df3-92c3e6653703.png) 

## Анализ участков вторичной стр-ры ДНК
Скачиваем два файла [plus](https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/blob/main/data/GSM3003539_Homo_all_w15_th-1_plus.hits.max.K.w50.25.bed.gz?raw=true), [minus](https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/blob/main/data/GSM3003539_Homo_all_w15_th-1_minus.hits.max.K.w50.25.bed.gz?raw=true) и "сливаем" их с помощью утилиты bedtools merge как было показано выше. В итоге получаем файл G4_seq.bed  
С помощью [этого](https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/raw/main/src/length_hist.R) и [этого](https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/raw/main/src/chip_seeker.R) кода получаем следующую гистограмму и график:  
![image](https://user-images.githubusercontent.com/55275328/121438396-9e196180-c98c-11eb-9aba-d4cfd289590b.png)  
![image](https://user-images.githubusercontent.com/55275328/121438360-948ff980-c98c-11eb-8886-d216d7b91307.png)  
Кол-во пиков: 428624

## Анализ пересечений гистоновой метки и стр-ры ДНК
Находим пересечения гистоновой метки с стр-рами ДНК с помощью данной команды:  
```
bedtools intersect  -a G4_seq.bed   -b  H3K4me1_GM23338.merge.hg19.bed  >  H3K4me1_GM23338.intersect_with_G4.bed
```
С помощью [этого](https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/raw/main/src/length_hist.R) и [этого](https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/raw/main/src/chip_seeker.R) кода получаем следующую гистограмму и график:  
![image](https://user-images.githubusercontent.com/55275328/121438687-20a22100-c98d-11eb-8ce5-0bbd5d02d5b6.png)  
![image](https://user-images.githubusercontent.com/55275328/121438705-2bf54c80-c98d-11eb-856c-c182810a68c6.png)  
Кол-во пиков: 78113

## Визуализируем в геномном браузере
К предыдущей команде добавляем данные строчки:  
```
track visibility=dense name="G4_seq"  color=0,200,0  description="G4_seq.bed"
https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/blob/main/data/G4_seq.bed?raw=true

track visibility=dense name="intersect_with_G4"  color=200,0,0  description="H3K4me1_GM23338.intersect_with_G4.bed"
https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/raw/main/data/H3K4me1_GM23338.intersect_with_G4.bed
```
В итоге получаем: ![image](https://user-images.githubusercontent.com/55275328/121381903-cf733c80-c94e-11eb-8413-460bd85a266d.png)
![image](https://user-images.githubusercontent.com/55275328/121486732-14908080-c9da-11eb-82a8-8992aa5e577d.png) chr1:100471589-100471622

## Ассоциируем полученные пересечения с ближайшими генами.
С помощью [этого](https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/raw/main/src/ChIPpeakAnno.R) кода получаем 2 файла: [H3K4me1_GM23338.intersect_with_G4.genes.txt](https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/raw/main/data/H3K4me1_GM23338.intersect_with_G4.genes.txt) и [H3K4me1_GM23338.intersect_with_G4.genes_uniq.txt](https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/raw/main/data/H3K4me1_GM23338.intersect_with_G4.genes_uniq.txt).  
Всего генов 6164 из них 4515 уникальны.

## GO анализ
Воспользуемся сайтом http://pantherdb.org/.  
Наиболее значимые: ![image](https://user-images.githubusercontent.com/55275328/121417642-cdbb7000-c972-11eb-8e1a-fd711409f14a.png)
Полный список находится [здесь](https://github.com/BaronFonMonc/hse21_H3K4me1_G4_human/raw/main/data/pantherdb_GO_analysis.txt).



