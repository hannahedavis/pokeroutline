class GameState():
    def __init__(self):
        self.money1=1000
        self.money2=1000
        self.pot=0
        self.hand1=[]
        self.hand2=[]
        self.deck=[]
        self.lastAction=None
    def getMoney(self,player):
        if player==1:
            return self.money1
        elif player==2:
            return self.money2
    def setMoney(self, cat, delta):
        if cat==0:
            self.pot+=delta
        elif cat==1:
            self.money1+=delta
        elif cat==2:
            self.money2+=delta
    def setLastAction(self,action,amount):
        self.lastAction=(action,amount)
    def getLastAction(self):
        return self.lastAction
    def getPot(self):
        return self.pot
    def NewRound(self):
        self.money1-=50
        self.money2-=50
        self.pot+=100
        self.makeDeck()
        self.hand1=[]
        self.hand2=[]
        self.deal(1)
        self.deal(2)
    def makeDeck(self):
        h=[i+"h" for i in ['A','K','Q','J','T','9','8','7','6','5','4','3','2']]
        c=[i+"c" for i in ['A','K','Q','J','T','9','8','7','6','5','4','3','2']]
        d=[i+"d" for i in ['A','K','Q','J','T','9','8','7','6','5','4','3','2']]
        s=[i+"s" for i in ['A','K','Q','J','T','9','8','7','6','5','4','3','2']]
        deck=h+c+d+s
        random.shuffle(deck)
        self.deck=deck
    def discard(self,player,cards):
        temp=[]
        if player==1:
            for value in range(5):
                if value not in cards:
                    temp.append(self.hand1[value])
            self.hand1=temp
        elif player==2:
            for value in range(5):
                if value not in cards:
                    temp.append(self.hand2[value])
            self.hand2=temp            
    def deal(self,player):
        if player==1:
            while len(self.hand1)<5:
                self.hand1.append(self.deck[0])
                self.deck=self.deck[1:]
        elif self.CheckPlayer(player):
            while len(self.hand2)<5:
                self.hand2.append(self.deck[0])
                self.deck=self.deck[1:]
    def getHand(self,player):
        if player==1:
            return self.hand1
        elif player==2:
            return self.hand2
class Tournament():
    def __init__(self,filename):
        import random
        self.filename=filename
        self.wins={}
        self.ifile=open(filename)
        self.names=self.ifile.readlines()
        self.name1=''
        self.name2=''
        for i in self.names:
            self.wins[i]=0
        self.done=[(i,i) for i in names]
    def nextpair(self):
        self.name1=random.choice(self.names)
        self.name2=random.choice(self.names)
        while (self.name1,self.name2) in done or (self.name2,self.name1) in done:
            self.name1=random.choice(names)
            self.name2=random.choice(names)
        print (self.name1,'vs.',self.name2)
        self.done.append((name1,name2))
        return(self.name1,self.name2)
    def runTournament(self)
        while len(self.done)<len(self.names)*(len(self.names)-1)//2:
            self.nextpair()
            import importlib
            self.name1=importlib.import_module(self.name1)
            self.name2=importlib.import_module(self.name2)
            player1=name1.Player(1)
            player2=name2.Player(2)
            gamestate=GameState()
            i=0
            while gamestate.getMoney(1)>50 and gamestate.getMoney(2)>50:
                i=i%2+1
                gamestate.NewRound()
                if i==1:
                    ex1=player1.revise(gamestate.gethand(1))
                    gamestate.discard(self,1,ex1)
                    gamestate.deal(self,1)
                    ex1=len(ex1)
                    ex2=player2.revise(gamestate.gethand(2))
                    gamestate.discard(self,2,ex2)
                    gamestate.deal(self,2)
                    ex2=len(ex2)
                elif i==2:
                    ex2=player2.revise(gamestate.gethand(2))
                    gamestate.discard(self,2,ex2)
                    gamestate.deal(self,2)
                    ex2=len(ex2)
                    ex1=player1.revise(gamestate.gethand(1))
                    gamestate.discard(self,1,ex1)
                    gamestate.deal(self,1)
                    ex1=len(ex1)
