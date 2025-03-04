@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor
@Getter
@Setter
public class Department {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true, nullable = false)
    private String name;

    @OneToMany(cascade = CascadeType.ALL)
    private List<Employee> employees;
}


@Entity
@Data
@NoArgsConstructor
@AllArgsConstructor

public class Employee {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "Employee name is required")
    private String name;

    @Min(value = 18, message = "Age must be at least 18")
    private int age;

    @ManyToOne
    @JoinColumn(name = "department_id", nullable = false)
    private Department department;

}

@RestController
@RequestMapping("/department")
public class DepartmentController {

    private final DepartmentService departmentService;


    @Autowired
    public DepartmentController(DepartmentService departmentService) {
        this.departmentService = departmentService;
    }

    @Transactional
    @PostMapping("/addDepartment")
    public ResponseEntity<Department> addDepartment(@Valid @RequestBody Department department) {
        return ResponseEntity.ok(departmentService.addDepartment(department));
    }

}

{
    "timestamp": "2025-03-04T08:14:29.909+00:00",
    "status": 500,
    "error": "Internal Server Error",
    "path": "/department/addDepartment"
}
Hibernate: create table department (id integer not null auto_increment, name varchar(255) not null, primary key (id)) engine=InnoDB
Hibernate: create table employee (id integer not null auto_increment, age integer not null, employee_id integer not null, name varchar(255) not null, department_id integer not null, primary key (id)) engine=InnoDB
Hibernate: alter table department drop index UK1t68827l97cwyxo9r1u6t4p7d
Hibernate: alter table department add constraint UK1t68827l97cwyxo9r1u6t4p7d unique (name)
Hibernate: alter table employee add constraint FKbejtwvg9bxus2mffsm3swj3u9 foreign key (department_id) references department (id)
2025-03-04T14:37:49.166+05:30  INFO 32864 --- [assessment] [           main] j.LocalContainerEntityManagerFactoryBean : Initialized JPA EntityManagerFactory for persistence unit 'default'
2025-03-04T14:37:50.011+05:30  WARN 32864 --- [assessment] [           main] JpaBaseConfiguration$JpaWebConfiguration : spring.jpa.open-in-view is enabled by default. Therefore, database queries may be performed during view rendering. Explicitly configure spring.jpa.open-in-view to disable this warning
2025-03-04T14:37:50.864+05:30  INFO 32864 --- [assessment] [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 8080 (http) with context path '/'
2025-03-04T14:37:50.880+05:30  INFO 32864 --- [assessment] [           main] c.cts.assessment.AssessmentApplication   : Started AssessmentApplication in 9.631 seconds (process running for 10.404)
2025-03-04T14:38:28.511+05:30  INFO 32864 --- [assessment] [nio-8080-exec-2] o.apache.coyote.http11.Http11Processor   : Error parsing HTTP request header
 Note: further occurrences of HTTP request parsing errors will be logged at DEBUG level.

java.lang.IllegalArgumentException: Invalid character found in method name [0x160x030x010x000xf70x010x000x000xf30x030x030x190x9al0xf30xf90x800x130xa90x800xef0x130xa50xbb0xaa0x89g$d{0x950x99(0xac0x99)0xdeJy0xa6?0x020x8d ]. HTTP method names must be tokens
	
