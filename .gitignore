#include "configuration.h"
#include <ctime>
#include <sys/time.h>
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
using namespace std;
using namespace std;
static gp order(gp);
class class1
{
public:
	gp gameboard[size];
	gp blank;

	class1()
    {
		blank=0;
	}

	~class1()
    {
	}
	bool load(class1 & active, int direction)
    {
		for(int i = 0; i < size; i++)
			gameboard[i] = active.gameboard[i] ;
		switch(direction)
        {
			case NORTH:
                 {
				blank = active.blank-width;
				break;
			}
			case SOUTH:
                 {
				blank = active.blank+width;
				break;
			}
			case EAST:
                  {
				blank = active.blank+1;
				break;
			}
			case WEST:
                 {
				blank = active.blank-1;
				break;
			}
		}
		gameboard[active.blank] = gameboard[blank];

		gameboard[blank] = 0;

		return true;
	}

	void printboard()
    {
        	for(int i = 0; i < size; i++){
			if(i % width == 0)
				cout << endl;
			gp tile = gameboard[i];
			if(tile >= 1000)
				cout << " ";
			else if (tile >= 100)
				cout << "  ";
			else if(tile >= 10)
				cout << "   ";
			else
				cout << "    ";

			if(tile==0){
				cout << "b";
			}
			else{
				cout << (int)tile;
			}
		}
		cout << endl;
		cout << "--------------------------------------------------" <<endl;
		out << "--------------------------------------------------" <<endl;

	}

};
void getBoard(class1 & gameboard);
class node {
public:
	class1 gameboard;
	node * parent;
	node * next;
	node * prev;
	unsigned char lastdir;
	unsigned char points;
	unsigned short movecost;
	unsigned short mandistance;
	unsigned int cost;
	unsigned int hashvalue;
	node(){
		points = 0;
	}

	~node(){
	}
	void printtraversal(){
		if(parent != NULL){
			parent->printtraversal();

		} else {
			cout << "";
		}
		cout << "move = " << movecost << "   cost = " << cost << "   hash = " << hashvalue << endl;
		gameboard.printboard();
		cout << endl;
	}

	void printmove()
    {
		if(testnodes)
        {
			int cost3 = mandistance*cnst3;
			int cost2 = misplacecount()*cnst2;
			int cost4 = cnst4 * prdistance();
			int cost5 = cnst5 * alterorder();
			int costSum = cost2 + cost3 + cost4;
			int cost1 = cost - costSum;
			//cout << "hash = " << hashvalue << endl;
			//cout << "number of moves = " << movecost << endl;
			//cout << "cost = " << cost  << " moves = "<< cost1 << " tiles out of place = "<< cost2 << " manhattandistance = "<< cost3 << " prdistance = " << cost4 << " alterorder = " << cost5  << endl;
		}

		gameboard.printboard();

	}

	void load(node & active, int direction)
    {
		gameboard.load(active.gameboard, direction);
		costcalc(&active);
	}
	void costcalc(node * active)
    {
		mandistance = manhattandistance();
		if(!nodehash)
        {
			hashvalue =  cnst2*misplacecount()
					+ cnst3*mandistance
					+ cnst4*prdistance()
					+ cnst5*alterorder();

			if(active == NULL)
            {
				movecost = 0;
				cost = cnst1 * movecost + hashvalue;
			}
             else
             {
				movecost = active->movecost+1;
				cost = cnst1 * movecost + hashvalue;
			}
		}
         else
         {
			if(active == NULL)
            {
				movecost = 1;
				cost = cnst1 * movecost
						+ cnst2*misplacecount()
						+ cnst3*mandistance
						+ cnst4*prdistance()
						+ cnst5*alterorder();

			}
            else
             {
				movecost = active->movecost+1;
				cost = cnst1 * movecost
						+ cnst2*misplacecount()
						+ cnst3*mandistance
						+ cnst4*prdistance()
						+ cnst5*alterorder();
			}
			hashvalue = 0;
			int modulus = sizeof(unsigned int)/(sizeof(gp));
			for(int i = 0; i < size; i ++)
            {
				int factor = 1;
				int offset = (i % modulus);
				for(int j = 0; j < offset; j++)
					factor  = factor * 256;
				hashvalue += gameboard.gameboard[i]*factor;
			}
		}
	}
	bool test_board_for_goal()
    {
		for(int i = 0; i < size; i++)
        {
			if (gameboard.gameboard[i] == i)
            {

			}
			else
            {
				return false;
			}
		}
		return true;
	}

	bool deadlock()
    {
		node * temp = (this);
		int counter = 0;

		while(true)
        {
			if(retest)
            {
				counter ++;
				temp = temp ->parent;
				if (temp == NULL)
					break;
				switch(counter)
                {
				case 12:
				case 30:
				case 40:
				case 56:
					if(matcher(*this,*temp))
						return true;
					break;
				default:
					if(counter %2 == 0)
						if(matcher(*this,*temp))
                        {
						return true;
						}
					break;
				}
			}
             else
              {
				temp = temp ->parent;
				if (temp == NULL)
					break;
				if(testnodes)
					temp->printmove();
				if(matcher(*this,*temp))
					return true;
			}
		}
		return false;


	}

	int manhattandistance()
    {
		int sum=0,val=0;
		for(int i=0;i<size;i++)
        {
			val = gameboard.gameboard[i];
			gp Trow = val / width;
			gp Tcol = val % width;
			gp Prow = i / width;
			gp Pcol = i % width;

			gp vertDist = 0;
			gp horzDist = 0;
			if(Tcol < Pcol)
				horzDist = Pcol-Tcol;
			else
				horzDist = Tcol-Pcol;
			if(Trow < Prow)
				vertDist = Prow - Trow;
			else
				vertDist = Trow - Prow;

			sum += (vertDist + horzDist);

		}
		return sum;
	}
int prdistance()
{
	int sum=0,val=0;
	for(int i=0;i<size;i++)
    {
		val = gameboard.gameboard[i];
		gp Trow = val / width;
		gp Tcol = val % width;
		gp Prow = i / width;
		gp Pcol = i % width;
		gp vertDist = 0;
		gp horzDist = 0;
		if(Tcol < Pcol)
			horzDist = Pcol-Tcol;
		else
			horzDist = Tcol-Pcol;
		if(Trow < Prow)
			vertDist = Prow - Trow;
		else
			vertDist = Trow - Prow;
		sum += order(val)*(vertDist + horzDist);
	}
	return sum;
}
int misplacecount()
{
	if(cnst2 > 0)
    {
	int sum=0;
		for(int i=0;i<size;i++)
        {
			if(gameboard.gameboard[i]!=i)
				sum = sum +1;
		}
		return sum;
	}
	return 0;
}
int alterorder()
{
	if(cnst5 > 0)
    {
	gp highestRank = order(size-1);
	cout << "pr" << highestRank << endl;
	bool complete = true;
	while(complete && highestRank > 0)
    {
		gp row = highestRank * width;
		for(gp col = 0; col< width; col++)
        {
			gp index = row + col;
			if(gameboard.gameboard[index] != index)
				complete = false;
		}
		gp col = width-1;
		for(gp index = col+row; index > 0; index -= width)
			if(gameboard.gameboard[index] != index)
				complete = false;
		if(complete == false)
			continue;
		highestRank--;
	}
	int cost = 0;
	for(gp row = 0; row <= highestRank; row ++){
		for(gp col = 0; col <= highestRank; col ++){
			gp index = row * width + col;
			gp value = gameboard.gameboard[index];
			if(value % width == 0)
				continue;
			gp neighbor = value -1;
			gp up = index - width;
			gp down = index + width;
			gp left = index -1;
			gp right = index + 1;
			if(row != 0)
				if(gameboard.gameboard[up] == neighbor)
					continue;
			if(col != 0)
				if(gameboard.gameboard[left] == neighbor)
					continue;
			if(col < width-1)
				if(gameboard.gameboard[right] == neighbor)
					continue;
			if(row < width -1)
				if(gameboard.gameboard[down] == neighbor)
					continue;
			cost += order(value);
		}
	}
	return cost;
	}
	return 0;
}
bool matcher(node & n1, node & n2)
{
	if((n1.hashvalue)!= (n2.hashvalue))
		return false;
	for(int i=0; i<size;i++)
    {
		if(n1.gameboard.gameboard[i] !=  n2.gameboard.gameboard[i])
        {
			return false;
		}
	}
	return true;
}
};
bool solutionchecker(node & node_input);
static gp order(gp tile)
{
	if(!dorank)
		return 1;
	gp row = tile/width;
	gp col = tile%width;
	if(row > col)
		return row;
	else
		return col;
}
class class2
{
	node *freelist;
	node *treelist;
	node * starter;
public:
	unsigned int brdcount;
	unsigned int blkcount;
	unsigned int prncount;
	class2()
    {
		treelist = NULL;
		freelist = NULL;
		brdcount=0;
		blkcount = 0;
		prncount = 0;
		starter = new node[ramsize];
	}
	~class2()
    {
		for(unsigned int i = 0; i < ramsize; i++)
        {
			node* rmv = &starter[i];
			if(rmv == treelist)
            {
				treelist = treelist->next;
				if(treelist != NULL)
					treelist->prev = NULL;
			}
            else if (rmv == freelist)
            {
				freelist = freelist->next;
				if(freelist !=NULL)
					freelist->prev = NULL;
			}
			if(rmv->prev != NULL)
				rmv->prev->next = rmv->next;
			if(rmv->next != NULL)
				rmv->next->prev = rmv->prev;
		}
		while(treelist != NULL)
        {
			node * rmv = treelist;
			treelist = treelist->next;
			delete rmv;
		}
		while(freelist != NULL)
        {
			node * rmv = freelist;
			freelist = freelist->next;
			delete rmv;
		}
		delete starter;
	}
	void reset()
    {
		while(treelist != NULL)
        {
			node * temp = treelist;
			treelist = treelist -> next;
			temp -> next = freelist;
			if(freelist != NULL)
				freelist->prev = temp;
			freelist = temp;
		}
		brdcount = 0;
	}
	node* nodeallocator()
    {
		if(blkcount >= maxramsize)
			return NULL;
		node * temp;
		if(stats)
			brdcount++;
		if(freelist == NULL)
        {
			if(blkcount < ramsize)
            {
				temp = &starter[blkcount];
				blkcount ++;
			}
            else
             {
				if(stats)
					blkcount++;
				try
                 {
					temp = new node();
				}
                 catch(exception &e)
                 {
					cout << "overload" << endl;
					return NULL;
				}
			}
			temp->next = treelist;
			if(temp->next != NULL)
				temp->next->prev = temp;
			temp->prev = NULL;
			treelist = temp;
		}
        else
        {
			temp = freelist;
			freelist = freelist->next;
			if(freelist != NULL)
				freelist->prev = NULL;
			temp->next = treelist;
			if(temp->next != NULL)
				temp->next->prev = temp;
			treelist = temp;
		}
		return temp;
	}
	void freenodeallocator(node * rec)
    {
		prncount++;
		if(rec->parent != NULL){
			if(rec->parent->points > 0)
				rec->parent->points--;
			if(rec->parent->points == 0)
            {
				freenodeallocator(rec->parent);
			}
			rec->points = 0;
		}
		if(rec == treelist)
        {
			treelist = treelist->next;
			treelist->prev = NULL;
		}
         else
          {
			rec->prev->next = rec->next;
			if(rec->next != NULL)
				rec->next->prev = rec->prev;
			rec->prev = NULL;
		}
		rec->next = freelist;
		if(freelist != NULL)
			rec->next->prev = rec;
		freelist = rec;
	}
};
typedef node* ndptr;
class cmpnd
{
public:
	cmpnd(){}
	bool operator()(const ndptr & l, const ndptr & m )const
	{
		return l->cost > m->cost;
	}
};
typedef priority_queue<ndptr, vector<ndptr>, cmpnd> useful;

class finding
{
public:
	bool sol;
	node * root;
	node * active;
	class2 * broker;
	node * updater;
	node * child;
	node n;
	useful exist;
	void setout(node* rootNode, class2* NodeBroker, struct timeval &start)
    {
		updater = NULL;
		sol = false;
		root = rootNode;
		broker = NodeBroker;
		active = root;
		bool stopSearching = false;
		exist.push(active);
		if(findBestSolution)
        {
		unsigned long count = 0;
			while(!exist.empty() && !stopSearching)
            {
				count ++;
				if(count % 10000 == 0)
                {
					struct timeval stop;
					gettimeofday(&stop, NULL);
					double seconds  = stop.tv_sec  - start.tv_sec;
					double useconds = stop.tv_usec - start.tv_usec;
					double mtime = ((seconds) * 1000 + useconds/1000.0) + 0.5;
					if (mtime > lifespan)
                    {
						stopSearching = true;
						continue;
					}
				}
				active = exist.top();
				exist.pop();
				if(active->test_board_for_goal())
                {
					if((updater == NULL) || active->movecost < updater->movecost)
                    {
						if(teststate)
                        {
							struct timeval stop;
							gettimeofday(&stop, NULL);
							double seconds  = stop.tv_sec  - start.tv_sec;
							double useconds = stop.tv_usec - start.tv_usec;
							double mtime = ((seconds) * 1000 + useconds/1000.0) + 0.5;
							if(updater == NULL)
								cout << "uppdated sol  moves = " << active->movecost << ", time taken " << mtime << endl;
							else
								cout << "updated sol moves = " << active->movecost << ",sol = " << updater->movecost << "time taken " << mtime << endl;
						}
						if(updater != NULL)
							broker->freenodeallocator(updater);
						updater = active;
						continue;
					}
                    else
                    {
						broker->freenodeallocator(active);
						continue;
					}
				}
                else
                {
					if(updater != NULL)
                    {
						unsigned int manhattandistance = active->movecost + active->mandistance;
						if(manhattandistance > updater->movecost)
                        {
							broker->freenodeallocator(active);
							continue;
						}
					}
					if (!(active->deadlock()) )
                    {
						if(newchild() < 0)
                        {
							stopSearching = true;
							continue;
						}
					}
                    else
                    {
						broker->freenodeallocator(active);
						if(chaintest)
							cout << "cycle exist" << endl;
					}
				}
				continue;
			}
			if(updater == NULL)
				cout << "no solution found" << endl;
			else
				updater->printtraversal();
		}
		else
		{
			while (sol == false && ! stopSearching)
            {
				if(exist.empty())
                {
					cout << "solution not found" << endl;
					break;
				}
				active = exist.top();
				exist.pop();
				cout<<endl;
				if(testnodes)
                {
					cout<< "----------------------------------------------" << endl;
					cout<<"*********************************************"<<endl;
					cout << "" << endl;
					active->printmove();
				}
				sol = active->test_board_for_goal();
				if (sol && !startuptme)
                {
						cout << "" << endl;
						active->printtraversal();
				}
				else
                {
					if (!(active->deadlock()) )
                    {
						if(newchild() < 0)
                        {
							stopSearching = true;
							continue;
						}
					}
					else
                    {
						broker->freenodeallocator(active);
						if(chaintest)
							cout << "cycle exist" << endl;
					}
				}
			}
		}
	}
	int newchild()
    {
		for (int k=1;k<=4;k++)
			if(possiblemove(k,active))
            {
				child = broker->nodeallocator();
				if(child == NULL)
					return -1;
				child -> load(*active,k);
				pqentry(k);
			}
		return 0;
	}
	void pqentry( int direction)
    {
		child->lastdir = direction;
		child->parent = active;
		child->parent->points++;
		if(testnodes)
			child ->printmove();
		exist.push(child);
	}
	bool possiblemove(int direction,node * active)
    {
			 gp rowEmpty = active->gameboard.blank / width;
			 gp colEmpty = active->gameboard.blank % width;
			 switch(direction)
             {
			 	case NORTH:
                     {
			 		if(rowEmpty == 0 || active->lastdir == 2 )
			 			return false;
			 		return true;
			 	}
			 	case SOUTH:
                      {
			 		if(rowEmpty == width-1 || active->lastdir == 1)
			 			return false;
			 		return true;
			 	}
			 	case EAST:
                     {
			 		if(colEmpty == width-1 || active->lastdir == 4)
			 			return false;
			 		return true;
			 	}
			 	case WEST:
                     {
			 		if(colEmpty == 0 || active->lastdir == 3)
			 			return false;
			 		return true;
			 	}
			 	default: return false;
			 }
		 }
};



int main(int argc, char *argv[]) {
	struct timeval start, stop, brokerStop;
    double seconds, useconds, brokerTime;
    gettimeofday(&start, NULL);
    class2 broker;
    gettimeofday(&brokerStop, NULL);
    seconds  = brokerStop.tv_sec  - start.tv_sec;
    useconds = brokerStop.tv_usec - start.tv_usec;
    brokerTime = ((seconds) * 1000 + useconds/1000.0) + 0.5;
	node * root = broker.nodeallocator();
		getBoard(root->gameboard);
	if(stats)
	    gettimeofday(&start, NULL);
	root->costcalc(NULL);
	root->parent = NULL;
	if(!solutionchecker(*root))
	{
		root->gameboard.printboard();
		cout << "cannot solve" <<endl;
		return 0;
	}


	finding Search;
	Search.setout(root,&broker,start);
	if(stats){
	    gettimeofday(&stop, NULL);

	    seconds  = stop.tv_sec  - start.tv_sec;
	    useconds = stop.tv_usec - start.tv_usec;

	    double mtime = ((seconds) * 1000 + useconds/1000.0) + 0.5;
	}

	return 0;
}



bool solutionchecker(node & node_input){
        int j =0, sum =0;
        for (int i = 0;i<size;i++){
            for(j=0 ; j<i ; j++){

                if((node_input.gameboard).gameboard[j] > (node_input.gameboard.gameboard[i]) ){
                    sum = sum + 1;

                }
            }
        }
        int row = node_input.gameboard.blank / width;
        int col = node_input.gameboard.blank % width;
        if(row % 2 == 0)
        	sum = sum + col % 2;
        else
        	sum = sum + col % 2 + 1;

        return sum%2 ==0;
    }



void getBoard(class1 & gameboard){
			gameboard.gameboard[0] = 7;
			gameboard.gameboard[1] = 8;
			gameboard.gameboard[2] = 6;
			gameboard.gameboard[3] = 5;
			gameboard.gameboard[4] = 1;
			gameboard.gameboard[5] = 3;
			gameboard.gameboard[6] = 2;
			gameboard.gameboard[7] = 4;
			gameboard.gameboard[8] = 0;
			gameboard.blank = 8;
}


