什么是链表？

首先，链表是一种线性的链式存储的数据结构，“链” 说明其特征，由一环一环也就是“节点”组成。
链表分三种：单链表、双向链表和循环链表。

单链表：节点1（Begin）->节点2->节点3->节点4->END

节点1为头，END为结束，也就说节点4为链表的尾 “->” 为链接的方式。
接下来我们来看代码的简单实现（进行了简单调试）。单链表为最简单的链表，我觉得重点在于"->"链接方式，掌握其链接方式可以用更加形象的图像形式理解。

链表与数组之间的差别在于，首先最明显的差别，数组在堆栈中的存储空间是连续的，链表是不连续的，这个节点在地址0x100，下一个节点可能跑到0x200去了。
因为链表的结构特性，其明显的优势在于插入，删除操作极其方便，而且快。同样的操作，用数组去实现就会显得复杂多。
但个人觉得，也有不够方便的地方，比如数组，我想用哪个就直接利用下标去取值即可，但是链表，你做不到，你必须遍历得去寻找他。
所以链表，数组，有利有弊，各自都有长处和短处，具体应用得看场合。毕竟大家都是一种数据的结构。 ^ ^   // ------ westzhao 2017/9/2

/*************************************************************************
	> File Name: list.c
	> Author: westzhao 
	> Created Time: 2017/08/30 Wed 22:36:36
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<malloc.h>

typedef struct NODE
{
	int value;
	struct NODE *next;
}list;


int initList(list **head, int value);
int insertTail(list **node, int value);
int insertNode(list **node, int value, int position);
int deleteNode(list **node, int position);
int deleteList(list **head);
int getListLen(list **node);
int printList(list **head);


/*
 * Function: initList
 * Author: westzhao
 */
int initList(list **head, int value)
{
	*head = (list *)malloc(sizeof(list));
	if(*head == NULL)
	{
		printf("head node alloc failed!\r\n");
		return -1;
	}
	
	(*head)->value = value;
	(*head)->next = NULL;

	return 1;
}


/*
 * Function: insertTail
 *		     insert new node at the tail of list
 * Author: westzhao
 */
int insertTail(list **node, int value)
{
	list *current;
	list *new;
	
	current = *node;
	
	/* find the last node */
	while(current->next != NULL && current != NULL)
	{
		current = current->next;
	}
	
	new = (list *)malloc(sizeof(list));
	if(new == NULL)
	{
		printf("new node alloc mem failed!\r\n");
		return -1;
	}
	
	new->value = value;
	new->next = NULL;

	if(current == NULL)
	{
		/* the list is empty add the head of the list */
		current = new;
	}
	else
	{
		current->next = new;
	}

	return 1;
}


/*
 * Function: insertNode
 *		     insert new node by the position
 * Author: westzhao
 */
int insertNode(list **node, int value, int position)
{
	int current_pos;
	list *new;
	list *before;
	list *current;
	
	if(position <= 0)
	{
		printf("position:%d error\r\n", position);
		return -1;
	}
	
	current = *node;
	if(current == NULL)
	{
		printf("head node empty\r\n");
		return -1;
	}
	
	new = (list *)malloc(sizeof(list));
	if(new == NULL)
	{
		printf("new node alloc mem failed!\r\n");
		return -1;
	}

	for(current_pos = position; current_pos>0; current_pos--)
	{
		if(current != NULL)
		{
			before = current;
			current = current->next;
		}
	}

	new->value = value;
	
	before->next = new;
	new->next = current;

	return 1;
}


/*
 * Function: deleteNode
 *		     delete the Node by position
 * Author: westzhao
 */
int deleteNode(list **node, int position)
{
	int current_pos;
	list *before;
	list *current;

	if(position == 0)
	{
		printf("position:%d error\r\n", position);
		return -1;
	}
	
	current = *node;
	if(current == NULL)
	{
		printf("head node empty\r\n");
		return -1;
	}
	
	for(current_pos = position; current_pos>0; current_pos--)
	{
		if(current != NULL)
		{
			before = current;
			current = current->next;
		}
		else
		{
			printf("pos:%d out of range\r\n", position);
			return -1;
		}
	}
	
	before->next = current->next;
	free(current);
	
	return 1;
}


/*
 * Function: deleteList
 *		     delete all of the list
 * Author: westzhao
 */
int deleteList(list **head)
{
	int pos;
	list *current;
	
	if(*head == NULL)
	{
		printf("list head NULL\r\n");
		return -1;
	}
	
	pos = getListLen(head);
	current = *head;
	
	while(pos > 1)
	{
		deleteNode(&current, pos-1);
		pos--;
	}
	
	free(current);
	*head = NULL;

	return 1;
}


/*
 * Function: getListLen
 *		     get length of the list
 * Author: westzhao
 */
int getListLen(list **node)
{
	int length = 0;
	list *current;
	
	if(*node == NULL)
	{
		printf("list empty");
		return -1;
	}

	current = *node;
	while(current != NULL)
	{
		current = current->next;
		length ++;
	}

	return length;
}


/*
 * Function: printList
 *		     print all of the list
 * Author: westzhao
 */
int printList(list **head)
{
	if(*head == NULL)
	{
		printf("list empty\r\n");
		return -1;
	}
	
	list *temp = *head;
	
	do
	{
		printf("%d\t", temp->value);
		temp = temp->next;
	}while(temp != NULL);

	printf("\r\n");

	return 1;
}


int main()
{
	list *head;
	
	initList(&head, 1);
    insertTail(&head, 12);
	insertTail(&head, 23);
	insertTail(&head, 100);
	insertNode(&head, 2, 1);
//	insertNode(&head, 2, 1);
//	insertNode(&head, 42, 2);
    //insertNode(&head, 12312, 7);
	printf("list len:%d \r\n", getListLen(&head));
    printList(&head);
	deleteNode(&head, 1);
	printf("list len:%d \r\n", getListLen(&head));
	printList(&head);
	deleteList(&head);
	printList(&head);

	return 0;
}