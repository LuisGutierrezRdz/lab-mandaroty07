# lab-mandaroty07

## Tasks
1. **Design Problem Solving:**

  * **Scenario Description:** Participants are provided with a series of common software design challenges. They will need to choose appropriate design patterns to solve these specific problems effectively.

  * **Design Challenges:**

    * **Global Configuration Management:** Design a system that ensures a single, globally accessible configuration object without access conflicts.

    * **Dynamic Object Creation Based on User Input:** Implement a system to dynamically create various types of user interface elements based on user actions.

    * **State Change Notification Across System Components:** Ensure components are notified about changes in the state of other parts without creating tight coupling.

    * **Efficient Management of Asynchronous Operations:** Manage multiple asynchronous operations like API calls which need to be coordinated without blocking the main application workflow.

  * **Task:** Outline solutions that integrate these patterns into a cohesive design to address the challenges.
    * **Global Configuration Management: Singletons With Enum
      ```
      public enum Singleton {
        INSTANCE;
      }
      ```
    * **Dynamic Object Creation Based on User Input:** Factory Pattern
      ```
      public class Rectangle implements Shape {

         @Override
         public void draw() {
            System.out.println("Inside Rectangle::draw() method.");
         }
      }  

      public class Square implements Shape {

         @Override
         public void draw() {
            System.out.println("Inside Square::draw() method.");
         }
      }

      public class Circle implements Shape {

         @Override
         public void draw() {
            System.out.println("Inside Circle::draw() method.");
         }
      }

      public class ShapeFactory {
	
          public Shape getShape(String shapeType){
            if(shapeType == null){
              return null;
            }		
            if(shapeType.equalsIgnoreCase("CIRCLE")){
              return new Circle();
            } else if(shapeType.equalsIgnoreCase("RECTANGLE")){
              return new Rectangle();
            } else if(shapeType.equalsIgnoreCase("SQUARE")){
              return new Square();
            }
      
            return null;
         }
      }
      ```
    * **State Change Notification Across System Components: Observer Pattern
      ```
      public class ONewsChannel implements Observer {

        private String news;

        @Override
        public void update(Observable o, Object news) {
          this.setNews((String) news);
        }  

        // standard getter and setter
      }

      public class ONewsAgency extends Observable {
        private String news;

        public void setNews(String news) {
            this.news = news;
            setChanged();
            notifyObservers(news);
        }
      }

      ONewsAgency observable = new ONewsAgency();
      ONewsChannel observer = new ONewsChannel();

      observable.addObserver(observer);
      observable.setNews("news");
      assertEquals(observer.getNews(), "news");
      ```
    * **Efficient Management of Asynchronous Operations: CompletableFuture
      ```
      public Future<String> calculateAsync() throws InterruptedException {
        CompletableFuture<String> completableFuture = new CompletableFuture<>();

        Executors.newCachedThreadPool().submit(() -> {
            Thread.sleep(500);
            completableFuture.complete("Hello");
            return null;
        });

        return completableFuture;
      }

      Future<String> completableFuture = calculateAsync();

      // ... 

      String result = completableFuture.get();
      assertEquals("Hello", result);
      ```
2- **Project Execution Simulation:**

  * Simulate the application of these patterns in a hypothetical software project. Document the approach, rationale, and integration process of the chosen patterns as they apply to the design challenges.

    Our system is an API to publish the exchange rate of the Mexican peso and the American dollar, the user will consume the API to obtain the values.

    We will expose a webhook so that we are notified when a value changes, the singleton pattern is used by the framework (Spring), we use the factory pattern to represent each change, in the webhook we use asynchronicity and the observer pattern to allow users to Subscribe to receive notifications.
    
