package com.cts.batchapp;

import com.cts.batchapp.config.BatchConfig;
import lombok.extern.slf4j.Slf4j;
import org.springframework.batch.core.configuration.annotation.EnableBatchProcessing;
import org.springframework.batch.core.job.builder.JobBuilder;
import org.springframework.batch.core.step.builder.StepBuilder;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@Slf4j
@SpringBootApplication
public class BatchappApplication implements ApplicationRunner {

	@Autowired
	private BatchConfig batchConfig;

	public static void main(String[] args) {
		SpringApplication.run(BatchappApplication.class, args);
	}
	@Override
	public void run(ApplicationArguments args) throws Exception {
		if(args.containsOption("jobName")){
			String jobName = args.getOptionValues("jobName").get(0);
			performJob(jobName);
		}
		else{
			log.info("There is no argument present, so we can't run the cron jobs");
		}
	}
	public void performJob(String jobName) throws Exception {
		batchConfig.job(jobName);

	}

}

package com.cts.batchapp.task;

import org.springframework.stereotype.Component;

@Component
public class PrintHelloTask{


    public void execute(String jobName) throws Exception {
        System.out.println("runnig task: Hello, jobName= "+ jobName);
    }
}

package com.cts.batchapp.config;

import com.cts.batchapp.task.PrintHelloTask;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.EnableScheduling;
import org.springframework.scheduling.annotation.Scheduled;

@EnableScheduling
@Configuration
@Slf4j
public class BatchConfig {

    private int count=0;

    @Autowired
    private PrintHelloTask printHelloTask;

    @Scheduled(cron = "0 * * * *")
    @Bean
    public void job(String jobName) throws Exception {
        log.info("Running jobName: "+ jobName + " , count = "+ count++);
        printHelloTask.execute(jobName);
    }


}
tell me what is wrong and why I can't do mvn clean install also, I'm trying to use Spring Scheduler and tried to create a cron job, also if you have any better coding example and approach please tell me that. Please give me answer in detail. 

