File* fp = NULL;
fp = fopen("文件路径"，“参数”)；
参数：w r a   w+ r+ a+  wb rb ab
fclose(文件指针)

按字符读写
int fputc(int ch, FILE * stream); 返回写入的字符或-1
int fgetc(FILE * stream); 返回写入的字符或-1
int feof(FILE * stream);检测光标是否在文件结尾

按字符串读写
int fputs(const char * str, FILE * stream); 成功0 失败-1
char * fgets(char * str, int size, FILE * stream); 成功成功读取的字符串 NULL:读到文件尾或出错

fprintf(fp, "%d %d %d\n", 1, 2, 3);


size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);

删除文件 重命名文件
 int remove(文件路径)
 int rename(旧文件路径，新文件路径)
