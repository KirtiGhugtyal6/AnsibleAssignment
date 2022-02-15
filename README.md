# AnsibleAssignment

---
# tasks file for testroles
- name: Install nginx
  apt:
    name: nginx
    update_cache: yes
    state: latest 
  become: yes

- name: copy nginx.conf
  template:
    src: webserver.conf
    dest: /etc/nginx/sites-available/default
    force: yes
  become: yes

- name: copy html 
  copy:
    src: blackdog
    dest: /var/www/
    force: yes
  become: yes

- name: new website
  file:
    src: /etc/nginx/sites-available/default
    dest: /etc/nginx/sites-enabled/default
    state: link
  become: yes
  notify: restart nginx
- name: Allow all access to tcp
  ufw:
    rule: allow
    port: '8081'
    proto: tcp
