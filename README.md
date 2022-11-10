# Test-Django-Docker-Https-Deploy

# To deploy

- Make sure to create `.env` from `.env.sample` and replace the key
- Get the **first certificate**:
  ```bash
    docker-compose -f docker-compose.deploy.yml run --rm certbot /opt/certify-init.sh
  ```
- Then need to stop the docker service first, then run again
  ```bash
  docker-compose -f docker-compose.deploy.yml down
  docker-compose -f docker-compose.deploy.yml up
  ```


HANDLING RENEW CERTIFICATE
- Create `renew.sh`
  ```bash
  vim ~/renew.sh
  ```

- Add the code bellow but need to replace **YOUR_PROJECT_DIR** to your project location
  ```bash
  #!/bin/sh
  set -e

  cd YOUR_PROJECT_DIR
  /usr/local/bin/docker-compose -f docker-compose.deploy.yml run --rm certbot certbot renew
  ```
- Run 
  ```bash
  chmod +x ~/renew.sh
  ```
- Run
  ```bash
  crontab -e
  ```

- Then add
  ```bash
  0 0 * * 6 sh /home/ec2-user/renew.sh
  ```



# Resources:
- Guide: https://londonappdeveloper.com/django-docker-deployment-with-https-using-letsencrypt/
- Video: https://www.youtube.com/watch?v=3_ZJWlf25bY&t=3182s&ab_channel=LondonAppDeveloper