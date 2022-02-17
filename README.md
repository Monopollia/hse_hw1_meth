# hse_hw1_meth
## 1 Скачайте любой из запусков и проведите анализ QC прочтений
### Epiblast 
*Ссылка на Colab:* https://colab.research.google.com/drive/1LOnav4HSXczDuBXoF8jRzys0Y-BhtvEd#scrollTo=CLLeQw9Mxab4
### Можно заметить, что некоторые параметры (например уровень повторений последовательностей) схож с образцами сеаквенирования ДНК (больше, чем с РНК).
![image](https://user-images.githubusercontent.com/61352475/154204812-b1fc1b20-44a4-45dc-8a65-f0488ad4e7a8.png)
### Также стоит отметить своеобразные output данные Per base sequence content, процентное соотношение нуклеотидов сильно отличается (см. ниже)
![image](https://user-images.githubusercontent.com/61352475/154205829-6ff5c632-2e35-4dae-9831-6b34f44c5d8a.png)
![image](https://user-images.githubusercontent.com/61352475/154211038-5e2c9f76-c10f-4b1c-a249-46d732a29276.png)
### Распределение необычной формы может указывать на загрязненную библиотеку. У нас же нормальное распределение, которое смещено, что указывает на некоторое систематическое смещение, которое не зависит от положения нуклеотидов. (Предупреждение выдается, если сумма отклонений от нормального распределения составляет более 15% от числа считанных данны)
![image](https://user-images.githubusercontent.com/61352475/154553651-b4f14cc4-705c-4d67-bdb1-7ddd7f3d2f80.png)
### MultiQ
![image](https://user-images.githubusercontent.com/61352475/154209693-9021e5e7-ea2e-4543-8689-ab27574847e3.png)
### Большое количество уникальных чтений во всех образцах (12%, 7% и 4% дупликаций).
### Общая статистика:
![image](https://user-images.githubusercontent.com/61352475/154209586-05b202b8-4e65-4f3b-9403-f531306e7bed.png)
## 2

*Ссылка на Colab:* https://colab.research.google.com/drive/1gmpmUDALrpHR3o81ci6Uuhml3-27tEbG#scrollTo=bAaOdZCKsVtX 
### a) Число ридов, закартированных на участки 11347700-11367700; 40185800-40195800
Образцы | chr11:11347700-11367700 |	chr11:40185800-40195800 
-|-|-
8cell |	1090 |	464
epiblast |	2328 |	1062 
ICM |	1456 |	630 

### b) Дедупликация файлов выравниваний
`! ls *pe.bam | xargs -P 4 -tI{} deduplicate_bismark  --bam  --paired  -o s_{} {}`

Образцы | процент дупл
-|-
8cell |	100-81,69=18,31% 
epiblast |	100-97,08=2,92% 
ICM |	100-90,92=9,08%

### c) Коллинг метилирования цитозинов (скрин того, что код завершён в коллабе)
![image](https://user-images.githubusercontent.com/61352475/154179362-abb64320-b992-4f7b-b209-962a2d4f94b7.png)

### d) Отчет в формате html (см в одноимённой папке, (1) это с M-bias)
#### Эти графики полезны для контроля качества. Уровень метилирования, должен быть почти постоянным, но часто показывает смещение на 5' и / или 3' конце считываний. 
### 8cell (CpG в среднем 44%)
![image](https://user-images.githubusercontent.com/61352475/154556728-4224ab2d-a3ae-4059-8ef0-fa3e3eb20c38.png)
![image](https://user-images.githubusercontent.com/61352475/154556859-0910c0a3-3bc6-4a9a-81c1-ede4cf306f0f.png)
####
#### На втором считывании процент CHH calls коллеблется намного сильнее, чем на первом.
### Epiblast (CpG в среднем 77%)
![image](https://user-images.githubusercontent.com/61352475/154557030-36c87ec1-5cc3-4b2f-93af-b93e8e7ef90c.png)
![image](https://user-images.githubusercontent.com/61352475/154193245-c1d5b5c2-c43e-4210-96f7-f4adaebbe072.png)
#### На втором считывании процент CHH calls коллеблется намного сильнее, чем на первом.
### ICM (CpG в среднем 23%)
![image](https://user-images.githubusercontent.com/61352475/154556110-d3211a5d-4b2e-43c2-b590-56edbb678d1b.png)
![image](https://user-images.githubusercontent.com/61352475/154555850-a03e420f-e240-4b35-bc4b-770d641a2726.png)
#### На втором считывании процент CHH call коллеблется намного сильнее, чем на первом.
### e) С помощью .bedgraph постройте гистограмму распределения метилирования цитозинов по хромосоме (отображение насколько часто метилируются цитозины в данном образце: по X процент метилированных цитозинов, по Y - частота)

`df = pd.read_csv("s_SRR5836473_1_bismark_bt2_pe.deduplicated.bedGraph", delimiter='\t', skiprows=1, header=None)`

`plt.figure(figsize=[10, 7])`

`plt.title("метилирование 8cell", fontsize=20)`

`plt.hist(df2[3], bins=100, density=True)`

`plt.show()`

#### 8cell
![image](https://user-images.githubusercontent.com/61352475/154180757-234a9ea4-44cc-4612-8e28-cf264ef32a24.png)
#### Epiblast
![image](https://user-images.githubusercontent.com/61352475/154180791-5a624760-117f-42df-9170-f6e6721b23bf.png)
#### ICM
![image](https://user-images.githubusercontent.com/61352475/154180815-a50d50ec-72c6-4882-b02e-11094ef2a340.png)
#### Mетильная группа 5-метилцитозина ассоциируется с важными биологическими эффектами, ДНК становится более хрупкой на этих участках, тк 5-метилцитозин более склонен к формированию иминотаутомера. Наиболее это выражено у второго образца, а наименее - у последнего.
### f) Визуализируйте уровень метилирования и покрытия
Митилирование:

`! bedGraphToBigWig s_SRR5836473_1_bismark_bt2_pe.deduplicated.bedGraph m.chrom.sizes cell8_methylation.bigWig.bw`

`! bedGraphToBigWig s_SRR3824222_1_bismark_bt2_pe.deduplicated.bedGraph m.chrom.sizes epiblast_methylation.bigWig.bw`

`! bedGraphToBigWig s_SRR5836475_1_bismark_bt2_pe.deduplicated.bedGraph m.chrom.sizes icm_methylation.bigWig.bw`

`! make_tracks_file --trackFiles cell8_methylation.bigWig.bw epiblast_methylation.bigWig.bw icm_methylation.bigWig.bw -o tracks.ini`

`! pyGenomeTracks --tracks tracks.ini --region chr11:3000000-4000000 -o image.png`

![image](https://user-images.githubusercontent.com/61352475/154185119-ec985884-510c-46ab-a7a6-d5c5a8de26fc.png)

Покрытие:(код такой-же, только вместо *methylation* ставим *coverage*)
![image](https://user-images.githubusercontent.com/61352475/154185186-4fc20964-a872-427a-bebe-1eb71c95c46c.png)




