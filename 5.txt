#include <stdio.h>
#include <stdlib.h>
#include <string.h>



struct Library
{
	char book_title[20];
	char writer[20];
	char year[20];
	char publishing_company[20];
	char genre[20];
	struct Library *next_library;
}; 


struct Library * file_in(struct Library *start_value)
{
	struct Library *loopLiked = start_value;

	char *strInFileName;
	FILE *inFile;

	strInFileName = "C:/input.txt";

	if ((inFile = fopen(strInFileName, "r")) == NULL) {
		printf("\n출력 파일 열기 실패\n");
		exit(1);
	}


	char buffer[5][100];

	while (!feof(inFile)) {

		fscanf(inFile, "%s %s %s %s %s", buffer[0], buffer[1], buffer[2], buffer[3],buffer[4]);
		char *out[5];
		for (int i = 0; i < 5; i++) {
			out[i] = malloc(sizeof(char) * (strlen(buffer[i])));
			strcpy(out[i], buffer[i]);
		}

		struct Library *newData = malloc(sizeof(struct Library) * 1);
		strcpy(newData->book_title, out[0]);
		strcpy(newData->writer, out[1]);
		strcpy(newData->year, out[2]);
		strcpy(newData->publishing_company, out[3]);
		strcpy(newData->genre, out[4]);
		newData->next_library = NULL;
		
		if (start_value == NULL) {
			start_value = newData;
			loopLiked = start_value;
		}
		else {
			loopLiked->next_library = newData;
			loopLiked = loopLiked->next_library;
		}


	}
	fclose(inFile);

	return start_value;
}

void file_out(struct Library *start_value)
{
	char *strOutFileName;
	FILE *outFile;

	strOutFileName = "C:/input.txt";

	if ((outFile = fopen(strOutFileName, "w")) == NULL) {
		printf("\n출력 파일 열기 실패\n");
		exit(1);
	}

	while (start_value != NULL) {
		if (start_value->next_library == NULL) {
			fprintf(outFile, "%s %s %s %s %s", start_value->book_title, start_value->writer, start_value->year, start_value->publishing_company, start_value->genre);
		}
		else {
			fprintf(outFile, "%s %s %s %s %s\n", start_value->book_title, start_value->writer, start_value->year, start_value->publishing_company, start_value->genre);
		}
		start_value = start_value->next_library;
	}


	fclose(outFile);

}


struct Library * add_list_1(struct Library *start_value)
{
	printf("\n추가할 도서를 입력하세요(도서명, 저자, 출판연도, 출판사명, 장르 입력을 띄어쓰기로 구분해서 입력하세요):");
	struct Library* loop_test;
	loop_test = start_value;

	while (loop_test->next_library != NULL)
	{

		loop_test= loop_test->next_library;
	}
	loop_test->next_library = malloc(sizeof(struct Library )* 1);
	scanf("%s %s %s %s %s", loop_test->next_library->book_title, loop_test->next_library->writer, loop_test->next_library-> year, loop_test->next_library->publishing_company, loop_test->next_library->genre);
	loop_test->next_library->next_library = NULL;
		
	
	return start_value;
}


void search_list_2(struct Library *start_value) // 검색어 입력받고 결과 출력해주는 함수
{
	char surf[20];
	struct Library* test = start_value;
	printf("\n검색어를 입력하세요(도서명, 저자, 출판연도, 출판사명, 장르 으로 검색가능): ");
	scanf("%s",&surf);
	printf("\n");
	printf("-----------------------------------\n");
	int count = 0;
	while(test!= NULL)
	{
		if (strstr(test->book_title,surf)!=NULL)
		{
			printf("%s, %s, %s, %s, %s\n", test->book_title, test->writer, test->year, test->publishing_company, test->genre);
			count++;
			test = test->next_library;
			continue;

		}
		else if (strstr(test->writer,surf) != NULL)
		{
			printf("%s, %s, %s, %s, %s\n", test->book_title, test->writer, test->year, test->publishing_company, test->genre);
			count++;
			test = test->next_library;
			continue;
		}
		else if (strstr(test->year, surf) !=NULL)
		{
			printf("%s, %s, %s, %s, %s\n", test->book_title, test->writer, test->year, test->publishing_company, test->genre);
			count++;
			test = test->next_library;
			continue;
		}
		else if (strstr(test->publishing_company,surf)!=NULL)
		{
			printf("%s, %s, %s, %s, %s\n", test->book_title, test->writer, test->year, test->publishing_company, test->genre);
			count++;
			test = test->next_library;
			continue;
		}
		else if (strstr(test->genre, surf)!=NULL)
		{
			printf("%s, %s, %s, %s, %s\n", test->book_title, test->writer, test->year, test->publishing_company, test->genre);
			count++;
			test = test->next_library;
			continue;
		}
		else
		{
			test = test->next_library;
		}
		

	}
	if (count == 0) 
	{
		printf("\n\n검색결과가 없습니다");
	}
	else 
	{
		printf("\n\n총검색결과는 %d개입니다.\n\n", count);
	}
	
}

struct Library * modify_list_3(struct Library *start_value)
{
	int select_number;
	char mfy_value[20];
	char modify_finder[20];
	struct Library* test = start_value;
	printf("\n\n수정할 책의 제목을 정확히 입력하세요.: ");
	scanf("%s", &modify_finder);
	printf("\n");
	printf("-----------------------------------\n");
	while (test != NULL)
	{
		if (strcmp(test->book_title, modify_finder) == 0)
		{
			printf("%s, %s, %s, %s, %s\n", test->book_title, test->writer, test->year, test->publishing_company, test->genre);
			while (1) 
			{
				printf("\n\n수정할 항목을 입력하세요\n(1:도서명,2:저자,3:출판연도,4:출판사명,5:장르)\n");
				scanf("&s", &select_number);
				switch (select_number)
				{
				case 1:
					printf("\n수정할 도서 제목 입력하세요:");
					scanf("%s", &mfy_value);
					strcpy(test->book_title ,mfy_value);
					break;
				case 2:
					printf("\n수정할 저자를 입력하세요:");
					scanf("%s", &mfy_value);
					strcpy(test->writer, mfy_value);
					break;
				case 3:
					printf("\n수정할 출판 연도를 입력하세요:");
					scanf("%s", &mfy_value);
					strcpy(test->year, mfy_value);
					break;
				case 4:
					printf("\n수정할 출판사명을 입력하세요:");
					scanf("%s", &mfy_value);
					strcpy(test->publishing_company , mfy_value);
					break;
				case 5:
					printf("\n수정할 장르를 입력하세요:");
					scanf("%s", &mfy_value);
					strcpy(test->genre ,mfy_value);
					break;
				default:
					printf("\n잘못된 입력입니다. 다시입력하세요:");
					continue;
					break;
				}
				break;
			}
			
			break;

		
		}
		else
		{
			test = test->next_library;
		}
	}
	if (test == NULL)
	{
		printf("\n해당 검색어가 목록에 없습니다.");
	}
	return start_value;
}

struct Library * delete_list_4(struct Library *start_value)
{
	char delete_finder[20];
	struct Library* test = start_value;
	struct Library* test_before = start_value;
	printf("\n삭제할 책의 제목을 정확히 입력하세요.: ");
	scanf("%s", &delete_finder);
	printf("\n");
	printf("-----------------------------------\n");

	while (test != NULL)
	{
		
		if (strcmp(test->book_title, delete_finder) == 0)
		{
			printf("%s, %s, %s, %s, %s\n", test->book_title, test->writer, test->year, test->publishing_company, test->genre);
			break;
		}

		else
		{
			if (test != start_value)
			{
				test_before = test_before->next_library;

			}
			test = test->next_library;
		}


	}
	if (test == start_value)
	{
		start_value = test->next_library;
		free(test);
		printf("\n\n삭제되었습니다.");
	}
	else
	{
		test_before->next_library = test->next_library;
		free(test);
		printf("\n\n삭제되었습니다.");
	}
	if (test == NULL)
	{
		printf("\n\n해당 도서가 목록에 없습니다.");
	}
	return start_value;
}

void show_list_5(struct Library *start_value)
{
	while (start_value != NULL)
	{
		printf("%s ", start_value->book_title);
		printf("%s ", start_value->writer);
		printf("%s ", start_value->year);
		printf("%s ", start_value->publishing_company);
		printf("%s ", start_value->genre);
		printf("\n");

		start_value = start_value->next_library;
	}

}

int main()
{
	struct Library *start_value = NULL;

	start_value = file_in(start_value);


	



	while (1)
	{
		int option;

		printf("1.도서 추가(도서명, 저자, 출판연도, 출판사명, 장르 입력) \n");
		printf("2.도서 검색(도서명, 저자, 출판연도, 출판사명, 장르(으)로 검색가능) \n");
		printf("3.도서 정보 수정 \n");	 
		printf("4.도서 삭제 \n");
		printf("5.현재 총 도서 목록 출력(도서명, 저자, 출판일, 출판사명, 장르 출력) \n");
		printf("6.저장 \n");
		printf("7.프로그램 나가기(자동저장) \n");
		printf("입력: ");
		scanf("%d", &option);

		switch (option)
		{
		case 1:
		 start_value =add_list_1(start_value);
		 printf("");
			break;
		case 2:
			search_list_2(start_value);
			break;
		case 3:
			start_value = modify_list_3(start_value);
			break;
		case 4:

			start_value = delete_list_4(start_value);
			break;
		case 5:
			show_list_5(start_value);
			printf("");
			break;
		case 6:
			file_out(start_value);
			break;
		case 7:
			file_out(start_value);
			return 0;
		default:
			printf("\n잘못된 입력입니다. 다시입력하세요:");
			continue;
			break;
		}
		

	}


}