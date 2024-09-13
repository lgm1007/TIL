# Spring Batch
### 배치란?
* 데이터를 실시간으로 처리하지 않고 **일괄적으로 모아서 처리하는 작업**
* "대량의 데이터를, 특정 시간에, 일괄적으로 처리한다."
  * 배치 애플리케이션이 필요한 이유
    * 대용량 데이터를 처리하게 되면 작업량이 많기 때문에 리소스 또한 많이 사용하게 된다.
    * 대규모 데이터 처리 작업은 리소스를 점유하고 주요 비즈니스 로직을 방해하지 않기 위해 특정 시간에 배치가 동작하도록 한다.
### 배치 애플리케이션 구현을 위한 고려 요소
* 대용량 데이터
    * 배치 애플리케이션은 대량의 데이터를 가져오거나, 전달하거나, 계산하는 등의 작업이 있어야 한다.
* 자동화
    * 특정 상황을 제외하고 통상적으로 사용자의 개입 없이 실행 가능해야 한다.
* 견고성
    * 잘못된 데이터를 충돌 또는 중단 없이 처리할 수 있어야 한다.
* 신뢰성
    * 로깅 혹은 알림을 통해 오류가 발생한 부분을 추적할 수 있어야 한다.
* 성능
    * 지정한 시간 내에 처리를 완료하거나 배치 애플리케이션과 동시에 수행하는 다른 애플리케이션을 방해하지 않아야 한다.
### 구성 요소
1. **Job**
   * Spring Batch의 가장 큰 실행 단위
   * 일괄처리(batch processing)를 할 작업 단위를 정의한다.
2. **Step**
   * Job은 Step들로 구성되어 있다.
   * Job을 구성하는 작업의 단위로, Step은 Tasklet 또는 Chunk-oriented Processing 으로 구성된다.
     * Tasklet
       * Step 안에서 실행되는 한 개의 작업
     * Chunk-oriented Processing
       * Step 안에서 실행되는 여러 작업을 한 번에 처리하는 방법
       * 대용량의 데이터를 처리할 때 유용
3. **ItemReader**, **ItemProcessor**, **ItemWriter**
   * ItemReader
     * Step에서 처리할 데이터를 읽어오는 역할
   * ItemProcessor
     * ItemReader에서 읽어온 데이터를 가공하는 역할
   * ItemWriter
     * 가공한 데이터를 DB에 저장하거나 파일로 출력하는 등의 역할
### 사용 방법
* Spring Batch를 사용하여 대량의 데이터를 처리하는 방법에 대해 설명한다.
1. Batch Job 생성
    * 가장 기본은 Batch Job을 생성하는 것이다.
    * Batch Job을 생성하기 위해서는 Job과 Step 객체가 필요하다.
      * Job은 Batch Job 자체를 나타내며, Step은 Batch Job이 처리하는 작업을 나타낸다.
    * Job 객체는 `JobBuilderFactory`를, Step 객체는 `StepBuilderFactory` 를 사용하여 생성한다.
```java
@Configuration
public class BatchConfig {
    
    @Autowired
    public JobBuilderFactory jobBuilderFactory;

    @Autowired
    public StepBuilderFactory stepBuilderFactory;
    
    @Bean
    public Job processJob() {
        return jobBuilderFactory.get("processJob")
                .incrementer(new RunIdIncrementer())
                .flow(orderStep())
                .end()
                .build();
    }
    
    @Bean
    public Step orderStep() {
        return stepBuilderFactory.get("orderStep")
                .<Order, Order>chunk(10)
                .reader(itemReader())
                .processor(itemProcessor())
                .writer(itemWriter())
                .build();
    }
    
    @Bean
    public ItemReader<Order> itemReader() {
        // ...
    }
    
    @Bean
    public ItemProcessor<Order, Order> itemProcessor() {
        // ...
    }
    
    @Bean
    public ItemWriter<Order> itemWriter() {
        // ...
    }
}
```
2. Reader, Processor, Writer 구현
    * 대량의 데이터를 처리하기 위해서 Reader, Processor, Writer 를 구현해야 한다.
    * `ItemReader`는 데이터를 읽어오는 역할을, `ItemProcessor`는 읽어온 데이터를 가공하거나 필터링하는 역할, `ItemWriter`는 가공된 데이터를 실제 저장소에 저장하는 역할을 한다.
```java
@Bean
public ItemReader<Order> itemReader() {
    JdbcCursorItemReader<Order> reader = new JdbcCursorItemReader<Order>();
    reader.setDataSource(dataSource);
    reader.setSql("SELECT order_id, order_date, customer FROM orders");
    reader.setRowMapper(new OrderRowMapper());
    return reader;
}

@Bean
public ItemProcessor<Order, Order> itemProcessor() {
    return new OrderItemProcessor();
}

@Bean
public ItemWriter<Order> itemWriter() {
    JdbcBatchItemWriter<Order> writer = new JdbcBatchItemWriter<Order>();
    writer.setItemSqlParameterSourceProvider(new BatchPropertyItemSqlParameterSourceProvider<Order>());
    writer.setSql("INSERT INTO processed_orders (order_id, order_date, customer) VALUES (:orderId, :orderDate, :customer)");
    writer.setDataSource(dataSource);
    return writer;
}
```
3. Step 실행
    * Batch Job에서 Step 객체를 정의하고 Reader, Processor, Writer를 구현한 뒤에 `JobLauncher`를 사용하여 Batch Job을 실행한다.
```java
@Autowired
JobLauncher jobLauncher;

@Autowired
Job processJob;

public void runBatchJob() {
    try {
        JobParameters jobParameters = new JobParametersBuilder()
            .addString("JobId", String.valueOf(System.currentTimeMillis()))
            .toJobParameters();
        JobExecution jobExecution = jobLauncher.run(processJob, jobParameters);
        System.out.println("Sprinb Batch Job completed successfully.");
    } catch (Exception e) {
        System.out.println("Spring Batch Job failed: " + e.getMessage());
    }
}
```
