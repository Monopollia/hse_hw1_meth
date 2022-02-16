# hse_hw1_meth
## 1 Скачайте любой из запусков и проведите анализ QC прочтений

##2
*Ссылка на Colab:*https://colab.research.google.com/drive/1gmpmUDALrpHR3o81ci6Uuhml3-27tEbG#scrollTo=bAaOdZCKsVtX 

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
8cell |	81,69 % 
epiblast |	97,08% 
ICM |	90,92%

### c) Коллинг метилирования цитозинов (скрин того, что код завершён в коллабе)
![image](https://user-images.githubusercontent.com/61352475/154179362-abb64320-b992-4f7b-b209-962a2d4f94b7.png)

### d) Отчет в формате html (см в одноимённой папке)

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




