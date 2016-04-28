/*Bellman Ford algoritması bilgisayarlar arasındaki ağlarda en hızlı ve maliyeti en düşük yolu bulmak için kullanılır. Algoritmanın mantığında, belirli bir graphımız vardır ve bu graphda bir kaynak noktamız ve hedef noktamız olur. Bu iki nokta arasındaki en kısa yol bulunurken öncelikle kaynak noktamız dışındaki tüm noktalara sonsuz maliyet değeri atanır. Daha sonra bu graphdaki tüm edgelerin bir listesi çıkarılır. Bunun sırası çok önemli değildir fakat kolay anlaşılması için sıralı olursa daha iyi olur. Daha sonra sonsuz olan maliyetler bu listedeki maliyetlerle karşılaştırılır ve daha küçük olan değer yeni maliyet olarak güncellenir. Bu işlem tüm noktalar için tekarlanır ve sonunda en kısa yollar bulunmuş olur. Örnek olarak verdiğim graphın çizilmiş halini de ek olarak yükledim oradan kontrol edebilirsiniz */


#include <iostream>
#include <cstdlib>
#include <cstring>
#include <iomanip>

using namespace std;

const int inf=9999;
//yeni veri tipleri oluşturduk edge ve graph 
struct Edge{
	int kaynak, hedef, uzaklik;
};
struct Graph{
	int V,E;
	Edge* edge;
};
//Graph çizmek için fonksiyon tanımlandı
struct Graph* graphCiz(int V, int E){
	struct Graph* graph = (struct Graph*) malloc(sizeof(struct Graph));
    graph->V = V;
    graph->E = E;
 
    graph->edge = (struct Edge*) malloc(graph->E * sizeof(struct Edge));
 
    return graph;
}
//diziyi yazdırmak için bir fonksiyon tanımlandı
void ArrYaz(int uzunluk[], int n)
{
    cout<<"Kaynaktan hedefe olan uzaklik:"<<endl;
    for (int i = 0; i < n; ++i)
        cout<< i<<" -> "<< uzunluk[i]<<endl;
}
//Bellman ford algoritması için bir fonksiyon tanımlandı
void BellmanFord(struct Graph* graph, int kaynak)
{
    int V = graph->V;
    int E = graph->E;
    int uzunluk[V];
 
    // kaynak noktadan sonrasındaki tüm noktalara olaz uzaklığı sonsuz olarak belirledik
    for (int i = 0; i < V; i++)
        uzunluk[i] = inf;
    uzunluk[kaynak] = 0;
 
    // en kısa yol bulundu
    for (int i = 1; i <= V - 1; i++)
    {
        for (int j = 0; j < E; j++)
        {
            int u = graph->edge[j].kaynak;
            int v = graph->edge[j].hedef;
            int uzaklik = graph->edge[j].uzaklik;
            if (uzunluk[u] != inf && uzunluk[u] + uzaklik <= uzunluk[v])
                uzunluk[v] = uzunluk[u] + uzaklik;
        }
    }
 
    // Negatif uzunluk olup olmadığı kontrol ediliyor
    for (int i = 0; i < E; i++)
    {
        int u = graph->edge[i].kaynak;
        int v = graph->edge[i].hedef;
        int uzaklik = graph->edge[i].uzaklik;
        if (uzunluk[u] != inf && uzunluk[u] + uzaklik < uzunluk[v])
            cout<<"Graph negatif deger donduruyor!";
    }
 
    ArrYaz(uzunluk, V);
 
    return;
}

int main()
{
    //örnek bir graph tanımı yaptık
    int V = 6; // noktaların sayısı
    int E = 9; // edgelerin sayısı
	struct Graph* graph = graphCiz(V, E);
 
    // Edgeleri ekliyoruz
    // (a-b) arası 0-1
    graph->edge[0].kaynak = 0;
    graph->edge[0].hedef = 1;
    graph->edge[0].uzaklik = 3;
 
    // (a-c) arası 0-2
    graph->edge[1].kaynak = 0;
    graph->edge[1].hedef = 2;
    graph->edge[1].uzaklik = 3;
 
    // (b-c) arası 1-2
    graph->edge[2].kaynak = 1;
    graph->edge[2].hedef = 2;
    graph->edge[2].uzaklik = 1;
 
    // (b-d) arası 1-3
    graph->edge[3].kaynak = 1;
    graph->edge[3].hedef = 3;
    graph->edge[3].uzaklik = 5;
 
    // (c-d) arası 2-3
    graph->edge[4].kaynak = 2;
    graph->edge[4].hedef = 3;
    graph->edge[4].uzaklik = 8;
 
    // (c-e) arası 2-4
    graph->edge[5].kaynak = 2;
    graph->edge[5].hedef = 4;
    graph->edge[5].uzaklik = 10;
 
    // (d-e) arası 3-4
    graph->edge[6].kaynak = 3;
    graph->edge[6].hedef = 4;
    graph->edge[6].uzaklik = 2;
 
    // (d-f) arası 3-5
    graph->edge[7].kaynak = 3;
    graph->edge[7].hedef = 5;
    graph->edge[7].uzaklik = 6;
    
    // (e-f) arası 4-5
    graph->edge[8].kaynak = 4;
    graph->edge[8].hedef = 5;
    graph->edge[8].uzaklik = 3;
    cout<<"baslangic adres numarasini giriniz(0-5 araliginda):"<<endl;
    int k;
	cin>>k;
	BellmanFord(graph, k);
 
    return 0;
}
