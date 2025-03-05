public EmployeeDTO getEmployeeByEmployeeId(Integer id) {
        if (id < 100000 || id > 999999) {
            throw new IllegalArgumentException("Employee ID must be a 6-digit number.");
        }
        
        return new EmployeeDTO((Employee) employeeRepository.findByEmployeeId(id))
                .orElseThrow(() -> new EmployeeNotFoundException("Employee not found with ID: " + id));
    }
