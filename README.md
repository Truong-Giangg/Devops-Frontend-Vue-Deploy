# Devops-Frontend-Vue-Deploy - https://devopsedu.vn/courses/devops-for-freshers/lesson/bai-9-trien-khai-cac-du-an-frontend/?page_tab=files
Add todolist user
```bash
adduser todolist
```
Give project the correct user
```bash
chown -R todolist:todolist /home/giang/projects/todolist
```
Other user not allow any permission
```bash
chmod 750 /home/giang/projects/todolist
```
Install nodejs and npm (Note: use exact version for specific project)
Build project, run project
```bash
npm run build
npm run serve
```
Instal nginx as a webserver for our project
(Optional) You can change default port here
```bash
vi sites-available/default
```
Config nginx run our project
```bash
echo "server {
  listen 8081;
  root /home/giang/projects/todolist/dist;
  index index.html;
  try_files \$uri \$uri/ /index.html;
}" > conf.d/todolist.conf; systemctl restart nginx
```
Add nginx user to group todolist
```bash
usermod -aG todolist www-data
```
Reload instead of restart to prevent other nginx application get interupted
```bash
nginx -s reload
```
# Same with prj vision
```bash
adduser vision
chown -R vision. /home/giang/projects/vision
chmod -R 750 /home/giang/projects/vision
su vision
npm install
```
Deploy by using service
```bash
echo "
[Service]
Type=simple
User=vision
Restart=on-failure
WorkingDirectory=/home/giang/projects/vision/
ExecStart=npm run start -- --port=3000
" > /lib/systemd/system/vision.service
systemctl daemon-reload
systemctl start vision
```
# Deploy frontend using PM2
```bash
npm install -g pm2
