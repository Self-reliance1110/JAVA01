#案例实践
```
public class Component {
    protected String name;
    protected double price;

    public Component(String name,double price)
    {
        this.name = name;
        this.price = price;
    }


    public void description()
    {
        StringBuilder sb = new StringBuilder();
        sb.append("Component:{ name : ");
        sb.append(name);
        sb.append(" price : ");
        sb.append(price);
        System.out.println(sb);

    }


}
public interface Workable {
    void work();
}
public class Cpu extends Component implements Workable{
    protected int coreNum;

    public Cpu(String name, double price,int coreNum) {
        super(name, price);
        this.coreNum = coreNum;
    }

    @Override
    public void work() {
        
    }
}


public class Disk extends Component implements Workable{

    protected int volume;
    public Disk(String name, double price,int volume) {
        super(name, price);
        this.volume = volume;
    }

    @Override
    public void work() {
        

    }
}

public class intelCpu extends Cpu implements Workable{
    public intelCpu(String name, double price, int coreNum) {
        super(name, price, coreNum);
    }


    @Override
    public void work() {
        
    }
}

public class AMDCpu extends Cpu{
    public AMDCpu(String name, double price, int coreNum) {
        super(name, price, coreNum);
    }
    @Override
    public void work() {
        
    }
}

public class Seagate extends Disk{
    public Seagate(String name, double price, int volume) {
        super(name, price, volume);
    }
    @Override
    public void work() {
        
    }
}

public class WestDigtal extends Disk{
    public WestDigtal(String name, double price, int volume) {
        super(name, price, volume);
    }
    @Override
    public void work() {
       
    }
}

public class Computer extends Component implements Workable{
    protected Cpu cpu;
    protected  Disk disk;
    public Computer(String name, double price,Cpu cpu,Disk disk) {
        super(name, price);
        this.cpu = cpu;
        this.disk = disk;
    }

    @Override
    public void work() {
        StringBuilder sb = new StringBuilder();
        
        System.out.println(sb);
    }
}

public class ComputerStore {
    public static void main(String[] args) {
        Cpu cpu1 = new AMDCpu("AMDR7",4000,8);
        Disk disk1 = new Seagate("Seagate 600",6000,99999);
        Computer com1 = new Computer("",9999,cpu1,disk1);
        com1.work();
    }
}

```
