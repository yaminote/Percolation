import edu.princeton.cs.algs4.*;
public class Percolation {
    private WeightedQuickUnionUF uf1;
    private WeightedQuickUnionUF uf2;
    private int size=0;
    private int top;
    private int bottom;
    public Percolation(int n)
    {
        uf1=new WeightedQuickUnionUF(n*n+1);
        uf2=new WeightedQuickUnionUF(n*n+2);
        size=n;
        top=n*n;
        bottom=n*n+1;
    }
    public void open(int row,int col)
    {
        int a=(row-1)*size;
        int b=a+col-1;
        uf1.union(b,top);
        if(b<size)
            uf2.union(b,bottom);
        if(b>=top-size)
            uf2.union(b,top);
        if(b-size>=0&&uf1.connected(b-size,top))
            uf2.union(b-size,b);
        if(b-size-1>=0&&uf1.connected(b-size-1,top))
            uf2.union(b-size-1,b);
        if(b-size+1>=0&&uf1.connected(b-size+1,top))
            uf2.union(b-size+1,b);
        if(b+size<top&&uf1.connected(b+size,top))
            uf2.union(b+size,b);
        if(b+size-1<top&&uf1.connected(b+size-1,top))
            uf2.union(b+size-1,b);
        if(b+size+1<top&&uf1.connected(b+size+1,top))
            uf2.union(b+size+1,b);
        if(b-1>=0&&uf1.connected(b-1,top))
            uf2.union(b-1,b);
        if(b+1<top&&uf1.connected(b+1,top))
            uf2.union(b+1,b);
    }
    public boolean isOpen(int row,int col)
    {
        int a=(row-1)*size;
        int b=a+col-1;
        if(uf1.connected(b,top))
            return true;
        return false;
    }
    public boolean isFull(int row,int col)
    {
        int a=(row)*size;
        int b=a+col;
        if(uf2.connected(b,top))
            return true;
        return false;
    }
   public  int numberOfOpenSites()
    {
        int c=0;
        for(int i=0;i<top;i++)
        {
            if(uf2.connected(i,top))
                c++;
        }
        return c;
    }
    public boolean percolates()
    { 
        return uf2.connected(top,bottom);
    }
    public static void main(String args[])
    {
        System.out.println("Enter size");
        int n=StdIn.readInt();
        Percolation p1=new Percolation(n);
        while(true)
        {
            StdOut.println("Enter site to be opened");
            int r=StdIn.readInt();
            int c=StdIn.readInt();
            p1.open(r,c);
            if(p1.percolates()){
                StdOut.println("System percolates");break;}
        }

    }
}
