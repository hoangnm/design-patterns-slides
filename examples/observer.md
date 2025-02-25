```java
    // Subject interface
    interface NewsAgency {
        void attach(NewsSubscriber subscriber);
        void detach(NewsSubscriber subscriber);
        void notifySubscribers();
    }

    // Concrete Subject
    class BBCNews implements NewsAgency {
        private List<NewsSubscriber> subscribers = new ArrayList<>();
        private String news;

        public void setNews(String news) {
            this.news = news;
            notifySubscribers();
        }

        @Override
        public void attach(NewsSubscriber subscriber) {
            subscribers.add(subscriber);
        }

        @Override
        public void detach(NewsSubscriber subscriber) {
            subscribers.remove(subscriber);
        }

        @Override
        public void notifySubscribers() {
            for(NewsSubscriber subscriber : subscribers) {
                subscriber.update(news);
            }
        }
    }

    // Observer interface
    interface NewsSubscriber {
        void update(String news);
    }

    // Concrete Observers
    class NewsChannel implements NewsSubscriber {
        private String channelName;

        public NewsChannel(String name) {
            this.channelName = name;
        }

        @Override
        public void update(String news) {
            System.out.println(channelName + " received news: " + news);
        }
    }

    // Usage example
    class Main {
        public static void main(String[] args) {
            BBCNews bbcNews = new BBCNews();

            NewsSubscriber channel1 = new NewsChannel("Channel 1");
            NewsSubscriber channel2 = new NewsChannel("Channel 2");
            NewsSubscriber channel3 = new NewsChannel("Channel 3");

            bbcNews.attach(channel1);
            bbcNews.attach(channel2);
            bbcNews.attach(channel3);

            bbcNews.setNews("Breaking: Important event occurred!");
            // Output:
            // Channel 1 received news: Breaking: Important event occurred!
            // Channel 2 received news: Breaking: Important event occurred!
            // Channel 3 received news: Breaking: Important event occurred!
        }
    }
    ```