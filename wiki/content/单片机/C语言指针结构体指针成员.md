---
Title: C语言指针结构体指针成员
Sort: -1
---
用法：
例1：
```
typedef struct json_red1{
	char *cmd;
	char *model;
	char *sid;
	char channel[4];
}json_rec;

int main(){

 char *s="abcd" ;
 myjson_rec = (json_rec*)os_zalloc(sizeof(json_rec)); //申请内存位置
 //申请内存
 myjson_rec->myjson_rec->cmd= (char* )os_malloc(sizeof(char)*strlen(s)+1);
 
 os_strncpy(myjson_rec->cmd,cmd->valuestring,strlen(s)+1);
 os_printf("123: %s\r\n", myjson_rec->cmd);
}

```
例2：

```
typedef struct json_red1{
	char *cmd;
	char *model;
	char *sid;
	char channel[4];
}json_rec;

int main(){

char *s="abcd" ;
myjson_rec->cmd= os_malloc(sizeof(char)+strlen(s)+1); //申请内存位置
myjson_rec->cmd=(char*)(myjson_rec+1);								  //申请内存 
os_strncpy(myjson_rec->cmd,cmd->valuestring,strlen(s)+1);
os_printf("123: %s\r\n", myjson_rec->cmd);
}
```
例3：

```
typedef struct json_red1{
	char *cmd;
	char *model;
	char *sid;
	char channel[4];
}json_rec;

int main(){

char *s="abcd" ;
myjson_rec = (json_rec)os_malloc(sizeof(json_rec)); //申请内存位置
myjson_rec->cmd=(char*)(myjson_rec+1);								  //申请内存 
os_strncpy(myjson_rec->cmd,cmd->valuestring,strlen(s)+1);
os_printf("123: %s\r\n", myjson_rec->cmd);
}
```