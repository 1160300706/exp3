# exp3
#include <iostream>
#include <stdio.h>
using namespace std;

#define N 17

typedef struct MTGragh
{
    int vertex [N]; //顶点表
    int edge[N][N];
//邻接矩阵—边表, 可视为边之间的关系
    int n, e; //图的顶点数与边数
} ;

typedef struct node  //边表结点
{
    int adjvex; //邻接点域（下标）
    int  cost; //边上的权值
    struct node *next; //下一边链接指针
} EdgeNode;
typedef struct  //顶点表结点
{
    int  vertex; //顶点数据域
    EdgeNode * firstedge;//边链表头指针
} VertexNode;
typedef struct  //图的邻接表
{
    VertexNode vexlist [N];
    int n, e;  //顶点个数与边数
} AdjGraph;

int CreateMGragh (MTGragh *G,int c[N]) //建立图的邻接矩阵
{
    int i, j, k, w;
    int flag;
    flag=c[0];
    G->n=c[1];
    for (i=0; i<G->n; i++) //2.读入顶点信息，建立顶点表
        G->vertex[i]=i;
    for (i=0; i<G->n; i++)
        for (j=0; j<G->n; j++)
            G->edge[i][j]=0; //3.邻接矩阵初始化
    int a,b;
    int temp=0;
    i=2;
    if(flag==0)
    {
        while(i<N)
        {
            a=c[i];
            i++;
            b=c[i];
            i++;
            G->edge[a][b] = c[i];
            G->edge[b][a] = c[i];
            temp++;
            i++;
        }
    }
    else
    {
        while(i<N)
        {
            a=c[i];
            i++;
            b=c[i];
            i++;
            G->edge[a][b] = c[i];
            //G->edge[b][a] = c[i];
            temp++;
            i++;
        }
    }
    G->e=temp;
    return G->e;
} //时间复杂度：T

void putMgraph(MTGragh G)
{
 for(int i=0; i<G.n; i++)
    {
        for(int j=0; j<G.n; j++)
        {
            cout<<G.edge[i][j]<<" ";
        }
        cout<<endl;
    }
}


void CreateGraph (AdjGraph G,int c[N],int k)
{
     G.n=c[1];
     G.e=k;    //1.输入顶点个数和边数
    for ( int i = 0; i < G.n; i++)  //2.建立顶点表
    {
        G.vexlist[i].vertex=i;  //2.1输入顶点信息
        G.vexlist[i].firstedge = NULL;//2.2边表置为空表
    }
    int j=2;
    for ( int i = 0; i < k; i++)//3.逐条边输入,建立边表
    {
        //cin >> tail >> head >> weight; //3.1输入
        int tail = c[j];
        j++;
        int head=c[j];
        j++;
        int weight = c[j];
        j++;
        EdgeNode * p = new EdgeNode;       //3.2建立边结点
        p->adjvex = head;
        p->cost = weight; //3.3设置边结点
        p->next = G.vexlist[tail].firstedge;  //3.4链入第 tail 号链表的前端
        G.vexlist[tail].firstedge = p;
        p = new EdgeNode;
        p->adjvex = tail;
        p->cost = weight;
        p->next = G.vexlist[head].firstedge; //链入第 head 号链表的前端
        G.vexlist[head].firstedge = p;
    }
} //时间复杂度：O(2e+n)

void putGraph(AdjGraph A)
{
    
}

int main()
{
    char  a[N]= {'0','4','0','1','6','0','2','1','0','3','5','1','2','5','2','3','5'};
    FILE *fpWrite=fopen("data.txt","w");
    char b[N];
    for(int i=0; i<N; i++)
    {
        fprintf(fpWrite,"%c",a[i]);
    }
    fclose(fpWrite);
    FILE *fpRead = fopen("data.txt","r");
    for(int i=0; i<N; i++)
    {
        fscanf(fpRead,"%c",&b[i]);
    }
    for(int i=0; i<N; i++)
        cout<<b[i]<<" ";
    cout<<endl;
    int data[N] = {0,4,0,1,6,0,2,1,0,3,5,1,2,5,2,3,5};
    MTGragh G;
    int k=CreateMGragh(&G,data);
    putMgraph(G);
    AdjGraph A;
    CreateGraph(A,data,k);
    putGraph(A);
    return 0;
}
