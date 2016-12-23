# serverspec-run-by-ansible-playbook
Serverspec run by ansible-playbook and it use hostvars variables of Ansible.

This playbook uses the following topics.

+ Ansible
  + [FAQ: In a template, get all the IPs of all machines in a group](https://support.ansible.com/hc/en-us/articles/201957817-In-a-template-get-all-the-IPs-of-all-machines-in-a-group)
+ Serverspec 
  + [advanced tips: How to use host specific properties](http://serverspec.org/advanced_tips.html)

![image01.png](https://qiita-image-store.s3.amazonaws.com/0/117986/ac3c31c1-e3c1-f421-71a2-bfa0509e6529.png)

### Env.
Host
- OSX　Yosemite
  - Ansible: 2.3.0 (devel 6c3de3c299) last updated 2016/12/22 
  - Serverspec: 2.37.2

Target hosts
 - 　Windows2012R2 *1
   - IIS
 - CentOS6.8 *2
   - apache

Directories and files

```
├── README.md
├── Rakefile                # Sererspec
├── hosts.yml               # Ansible
├── spec
│   ├── AAA                 # Sererspec
│   │   └── sample_spec.rb  # Sererspec
│   ├── BBB                 # Sererspec
│   │   └── sample_spec.rb  # Sererspec
│   └── spec_helper.rb      # Sererspec
├── spec.yml                # Ansible
├── spec_vars               # Ansible:　This is created when the playbook is runed.
│   ├── hostvars_centos6-httpd01.yml 
│   ├── hostvars_centos6-httpd02.yml
│   ├── hostvars_win2012-iis01.yml
└── templates               
    └── dump_variables.j2　　　　　　# Ansible
```

### Results

```
$ ansible-playbook -i hosts.yml spec.yml

PLAY [all] ******************************************************************************************************************************************************

TASK [create spec_vars directory] *******************************************************************************************************************************
changed: [win2012-iis01 -> localhost]
ok: [centos6-httpd02 -> localhost]
ok: [centos6-httpd01 -> localhost]

TASK [dump_variables hostvars to yml] ***************************************************************************************************************************
changed: [centos6-httpd02 -> localhost]
changed: [centos6-httpd01 -> localhost]
changed: [win2012-iis01 -> localhost]

TASK [rake serverspec with hostvars] ****************************************************************************************************************************
changed: [centos6-httpd02 -> localhost]
changed: [centos6-httpd01 -> localhost]
changed: [win2012-iis01 -> localhost]

TASK [stdout of serverspec] *************************************************************************************************************************************
ok: [centos6-httpd02] => {
    "raw_result.stdout_lines": [
        "/System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/bin/ruby -I/Library/Ruby/Gems/2.0.0/gems/rspec-support-3.5.0/lib:/Library/Ruby/Gems/2.0.0/gems/rspec-core-3.5.4/lib /Library/Ruby/Gems/2.0.0/gems/rspec-core-3.5.4/exe/rspec --pattern spec/BBB/\\*_spec.rb", 
        "", 
        "Command \"hostname\"", 
        "  stdout", 
        "    should contain \"centos6-httpd02\"", 
        "", 
        "Finished in 5.33 seconds (files took 2.5 seconds to load)", 
        "1 example, 0 failures"
    ]
}
ok: [centos6-httpd01] => {
    "raw_result.stdout_lines": [
        "/System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/bin/ruby -I/Library/Ruby/Gems/2.0.0/gems/rspec-support-3.5.0/lib:/Library/Ruby/Gems/2.0.0/gems/rspec-core-3.5.4/lib /Library/Ruby/Gems/2.0.0/gems/rspec-core-3.5.4/exe/rspec --pattern spec/AAA/\\*_spec.rb", 
        "", 
        "Service \"httpd\"", 
        "  should be enabled", 
        "  should be running", 
        "", 
        "Finished in 5.62 seconds (files took 2.68 seconds to load)", 
        "2 examples, 0 failures"
    ]
}
ok: [win2012-iis01] => {
    "raw_result.stdout_lines": [
        "/System/Library/Frameworks/Ruby.framework/Versions/2.0/usr/bin/ruby -I/Library/Ruby/Gems/2.0.0/gems/rspec-support-3.5.0/lib:/Library/Ruby/Gems/2.0.0/gems/rspec-core-3.5.4/lib /Library/Ruby/Gems/2.0.0/gems/rspec-core-3.5.4/exe/rspec --pattern spec/AAA/\\*_spec.rb", 
        "", 
        "Service \"W3SVC\"", 
        " WARN  WinRM::WinRMWebService : WinRM::WinRMWebService#run_powershell_script is deprecated. Use WinRM::CommandExecutor#run_powershell_script instead", 
        "  should be enabled", 
        " WARN  WinRM::WinRMWebService : WinRM::WinRMWebService#run_powershell_script is deprecated. Use WinRM::CommandExecutor#run_powershell_script instead", 
        "  should be running", 
        "", 
        "Finished in 9.7 seconds (files took 2.48 seconds to load)", 
        "2 examples, 0 failures"
    ]
}

PLAY RECAP ******************************************************************************************************************************************************
centos6-httpd01            : ok=4    changed=2    unreachable=0    failed=0   
centos6-httpd02            : ok=4    changed=2    unreachable=0    failed=0   
win2012-iis01              : ok=4    changed=3    unreachable=0    failed=0   
$ 
```
