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

