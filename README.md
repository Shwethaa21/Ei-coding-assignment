Design patterns in action with unique, creative, self-contained use cases.
I’ll prepare six Java examples: two behavioral, two creational, and two structural.

. Behavioral Patterns
(a) Observer Pattern – Stock Market Price Alerts
// Observer Pattern: Stock price change triggers alerts

interface StockObserver {
    void update(String stock, double price);
}

class MobileApp implements StockObserver {
    @Override
    public void update(String stock, double price) {
        System.out.println("📱 Mobile Alert: " + stock + " is now $" + price);
    }
}

class WebDashboard implements StockObserver {
    @Override
    public void update(String stock, double price) {
        System.out.println("💻 Web Dashboard: " + stock + " is now $" + price);
    }
}

class Stock {
    private final String symbol;
    private final java.util.List<StockObserver> observers = new java.util.ArrayList<>();
    private double price;

    public Stock(String symbol, double price) {
        this.symbol = symbol;
        this.price = price;
    }

    public void addObserver(StockObserver o) { observers.add(o); }
    public void setPrice(double newPrice) {
        this.price = newPrice;
        for (StockObserver o : observers) o.update(symbol, newPrice);
    }
}

public class ObserverDemo {
    public static void main(String[] args) {
        Stock apple = new Stock("AAPL", 150);
        apple.addObserver(new MobileApp());
        apple.addObserver(new WebDashboard());
        apple.setPrice(155.50); // triggers both observers
    }
}

(b) Strategy Pattern – Payment Gateway Switching
// Strategy Pattern: Different payment methods

interface PaymentStrategy {
    void pay(double amount);
}

class CreditCardPayment implements PaymentStrategy {
    public void pay(double amount) { System.out.println("💳 Paid $" + amount + " using Credit Card"); }
}

class PayPalPayment implements PaymentStrategy {
    public void pay(double amount) { System.out.println("🅿️ Paid $" + amount + " using PayPal"); }
}

class ShoppingCart {
    private PaymentStrategy strategy;
    public void setPaymentStrategy(PaymentStrategy strategy) { this.strategy = strategy; }
    public void checkout(double amount) { strategy.pay(amount); }
}

public class StrategyDemo {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();
        cart.setPaymentStrategy(new CreditCardPayment());
        cart.checkout(99.99);

        cart.setPaymentStrategy(new PayPalPayment());
        cart.checkout(49.50);
    }
}

🔹 2. Creational Patterns
(c) Singleton Pattern – Printer Spooler
// Singleton Pattern: Only one printer spooler instance

class PrinterSpooler {
    private static PrinterSpooler instance;
    private PrinterSpooler() {}
    public static synchronized PrinterSpooler getInstance() {
        if (instance == null) instance = new PrinterSpooler();
        return instance;
    }
    public void print(String doc) {
        System.out.println("🖨 Printing: " + doc);
    }
}

public class SingletonDemo {
    public static void main(String[] args) {
        PrinterSpooler spooler1 = PrinterSpooler.getInstance();
        PrinterSpooler spooler2 = PrinterSpooler.getInstance();
        spooler1.print("Report.pdf");
        System.out.println("Same instance? " + (spooler1 == spooler2));
    }
}

(d) Factory Method Pattern – Shape Factory
// Factory Method: Creating shapes dynamically

interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() { System.out.println("⚪ Drawing Circle"); }
}

class Square implements Shape {
    public void draw() { System.out.println("⬛ Drawing Square"); }
}

class ShapeFactory {
    public static Shape createShape(String type) {
        switch (type.toLowerCase()) {
            case "circle": return new Circle();
            case "square": return new Square();
            default: throw new IllegalArgumentException("Unknown shape type");
        }
    }
}

public class FactoryDemo {
    public static void main(String[] args) {
        Shape s1 = ShapeFactory.createShape("circle");
        Shape s2 = ShapeFactory.createShape("square");
        s1.draw();
        s2.draw();
    }
}

🔹 3. Structural Patterns
(e) Adapter Pattern – USB to Type-C Adapter
// Adapter Pattern: Legacy USB device works with Type-C port

interface TypeCPort {
    void connectWithTypeC();
}

class LegacyUSB {
    public void connectWithUSB() { System.out.println("🔌 Connected via USB"); }
}

class USBToTypeCAdapter implements TypeCPort {
    private final LegacyUSB usbDevice;
    public USBToTypeCAdapter(LegacyUSB usbDevice) { this.usbDevice = usbDevice; }
    public void connectWithTypeC() {
        System.out.println("Using Adapter...");
        usbDevice.connectWithUSB();
    }
}

public class AdapterDemo {
    public static void main(String[] args) {
        LegacyUSB usb = new LegacyUSB();
        TypeCPort adapter = new USBToTypeCAdapter(usb);
        adapter.connectWithTypeC();
    }
}

(f) Decorator Pattern – Coffee Shop Add-ons
// Decorator Pattern: Add-ons for coffee orders

interface Coffee {
    String getDescription();
    double cost();
}

class BasicCoffee implements Coffee {
    public String getDescription() { return "☕ Basic Coffee"; }
    public double cost() { return 2.0; }
}

class MilkDecorator implements Coffee {
    private final Coffee coffee;
    public MilkDecorator(Coffee coffee) { this.coffee = coffee; }
    public String getDescription() { return coffee.getDescription() + " + Milk"; }
    public double cost() { return coffee.cost() + 0.5; }
}

class SugarDecorator implements Coffee {
    private final Coffee coffee;
    public SugarDecorator(Coffee coffee) { this.coffee = coffee; }
    public String getDescription() { return coffee.getDescription() + " + Sugar"; }
    public double cost() { return coffee.cost() + 0.2; }
}

public class DecoratorDemo {
    public static void main(String[] args) {
        Coffee coffee = new BasicCoffee();
        coffee = new MilkDecorator(coffee);
        coffee = new SugarDecorator(coffee);
        System.out.println(coffee.getDescription() + " => $" + coffee.cost());
    }
}
