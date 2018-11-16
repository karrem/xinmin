# 数据保存为二进制文件并导出csv文件

## Getting Super Powers

Becoming a super hero is a fairly straight forward process:

```c
static double dataPtr[1000];
static double csvdataPtr[1000];
char vcsv[30];
char csv[100];
char Path[100];
char vPath[100];
char csvPath[100];
int i, j, rows=0;

if (access("/home/ftp/data/csvprint", 0) != 0) {
    debug_printf("path=%s\n", "new/home/ftp/data/csvprint");
    system("mkdir /home/ftp/data/csvprint");
}
if (access("/home/ftp/data/print", 0) != 0) {
    debug_printf("path=%s\n", "new/home/ftp/data/print");
    system("mkdir /home/ftp/data/print");
}
strcpy($file.filenamenow, "test.bw");
sprintf(Path, "%s/%s/%s", hmi_get_usr_data_dir(), "print", $file.filenamenow);
sprintf(vPath, "%s/%s/%s%s", hmi_get_usr_data_dir(), "csvprint", $file.filenamenow, ".CSV");
//debug_printf("Path=%s\n", Path); 

if (access(vPath, 0) == 0) {
    remove(vPath);
}
memset(dataPtr, 0, sizeof(dataPtr));
memset(csvdataPtr, 0, sizeof(csvdataPtr));

if (access(Path, 0) == 0) {  //存在
    debug_printf("tip=%s\n", "start");

    FILE *pFile = NULL;
    pFile = fopen(Path, "rb+");
    if (pFile != NULL)
	{

        fread(&rows, sizeof(int), 1, pFile);
        fread(&dataPtr[0], sizeof(double), 2*rows, pFile);  
        debug_printf("tip=%s\n", "read");
        fclose(pFile);
	}
    debug_printf("rows=%d\n", rows);
    FILE *pFile1 = NULL;
    pFile1 = fopen(Path, "wb+");
    if (pFile1 != NULL)
	{
        if (rows >= 500) {
            rows = 500;
            memmove(&dataPtr[2], &dataPtr[0], sizeof(double)*(2*(rows-1)));
        }else {
            rows++;
            memmove(&dataPtr[2], &dataPtr[0], sizeof(double)*(2*rows));
        }
        dataPtr[0] = (double)$system.CurDateTime;
    	dataPtr[1] = (double)$data.重量读取值;
        debug_printf("tip=%s\n", "write");
        fwrite(&rows, sizeof(int), 1, pFile1);
        fwrite(&dataPtr, sizeof(double), 2*rows, pFile1);
        fclose(pFile1);
    }
}else {
    FILE *pFile2 = NULL;
    pFile2 = fopen(Path, "wb+");
    if (pFile2 != NULL)
	{
		dataPtr[0] = (double)$system.CurDateTime;
        dataPtr[1] = (double)$data.重量读取值;
        debug_printf("tip=%s\n", "new"); 
        rows = 1;
        fwrite(&rows, sizeof(int), 1, pFile2);
		fwrite(&dataPtr, sizeof(double), 2, pFile2);
		fclose(pFile2);
    }
}

int year, month, day, hour, minu, sec; 

FILE *pFilec = NULL;
FILE *pFile3 = NULL;
pFile3 = fopen(vPath, "w+");
pFilec = fopen(Path, "rb+");
if (pFilec != NULL)
{
    memset(csv, 0, sizeof(csv));
    fread(&rows, sizeof(int), 1, pFilec);
    for (i=1; i<=2*rows; i++) {
            memset(vcsv, 0, sizeof(vcsv));
            fread(&csvdataPtr[i-1], sizeof(double), 1, pFilec);
            if (i%2==0) {//偶数
                sprintf(vcsv, "%f\n",csvdataPtr[i-1]);   
            }else {
                gettimeinfo(csvdataPtr[i-1], &year, &month, &day, &hour, &minu, &sec);
                sprintf(vcsv, "%4d-%2d-%2d %2d:%2d:%2d", year, month, day, hour, minu, sec);
                strcat(vcsv, ",");
            }    
            //strcat(csv, vcsv);  
            if (pFile3 != NULL)
            {
        	   fwrite(&vcsv, sizeof(vcsv), 1, pFile3);
            }
            debug_printf("vcsv=%s\n", vcsv);  
            //debug_printf("csv=%s\n", csv);   
    }
    fclose(pFile3);
    fclose(pFilec);
}
```

{% hint style="info" %}
 Super-powers are granted randomly so please submit an issue if you're not happy with yours.
{% endhint %}

Once you're strong enough, save the world:

```
// Ain't no code for that yet, sorry
echo 'You got to trust me on this, I saved the world'
```



