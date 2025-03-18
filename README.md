Parameter 1 of method job1 in com.cts.batchapp.config.BatchConfig required a single bean, but 3 were found:
	- step1: defined by method 'step1' in class path resource [com/cts/batchapp/config/BatchConfig.class]
	- step2: defined by method 'step2' in class path resource [com/cts/batchapp/config/BatchConfig.class]
	- step3: defined by method 'step3' in class path resource [com/cts/batchapp/config/BatchConfig.class]

package com.cts.batchapp.config;

import com.cts.batchapp.task.PrintHelloTask;
import org.springframework.batch.core.Job;
import org.springframework.batch.core.Step;
import org.springframework.batch.core.job.builder.JobBuilder;
import org.springframework.batch.core.repository.JobRepository;
import org.springframework.batch.core.step.builder.StepBuilder;
import org.springframework.batch.core.step.tasklet.Tasklet;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.transaction.PlatformTransactionManager;

@Configuration
public class BatchConfig {

    @Autowired
    private PrintHelloTask printHelloTask;

    @Bean
    public Step step1(JobRepository jobRepository, Tasklet task, PlatformTransactionManager transactionManager){
        return new StepBuilder("step1", jobRepository)
                .tasklet(task, transactionManager)
                .build();
    }
    @Bean
    public Step step2(JobRepository jobRepository, Tasklet task, PlatformTransactionManager transactionManager){
        return new StepBuilder("step2", jobRepository)
                .tasklet(task, transactionManager)
                .build();
    }
    @Bean
    public Step step3(JobRepository jobRepository, Tasklet task, PlatformTransactionManager transactionManager){
        return new StepBuilder("step3", jobRepository)
                .tasklet(task, transactionManager)
                .build();
    }

    @Bean
    public Job job1(JobRepository jobRepository, Step step, PlatformTransactionManager platformTransactionManager){
        return new JobBuilder("job1", jobRepository)
                .start(step1(jobRepository, printHelloTask, platformTransactionManager))
                .next(step2(jobRepository, printHelloTask, platformTransactionManager))
                .next(step3(jobRepository, printHelloTask, platformTransactionManager))
                .build();

        // next method is transition 
    }


}
