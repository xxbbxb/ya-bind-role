build-doсker-image: clean
	docker build -t "baseimage-ssh:latest" .

start-containers: build-doсker-image
	docker run --detach baseimage-ssh:latest > tmp/master
	docker inspect --format '{{ .NetworkSettings.IPAddress }}' $$(cat tmp/master) > tmp/master-ip
	docker run --detach baseimage-ssh:latest > tmp/slave
	docker inspect --format '{{ .NetworkSettings.IPAddress }}' $$(cat tmp/slave) > tmp/slave-ip

sandbox: start-containers
	curl -o tmp/insecure_key -fSL "https://github.com/phusion/baseimage-docker/raw/master/image/services/sshd/keys/insecure_key"
	chmod 600 tmp/insecure_key
	sed "s/ns1.*/ns1 ansible_ssh_host=$$(cat tmp/master-ip)/g" hosts | sed "s/ns2.*/ns2 ansible_ssh_host=$$(cat tmp/slave-ip)/g" > tmp/hosts-fixed
	sudo ansible-playbook bind.yml -i tmp/hosts-fixed --private-key=tmp/insecure_key

clean:
	docker stop $$(cat tmp/master) $$(cat tmp/slave) || true
	rm -rf tmp/*
	echo "for git" tmp/.keep
