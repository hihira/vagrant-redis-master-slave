# -------------------------------------------------------------------
# Copyright (c) 2015 Mark deVilliers.  All Rights Reserved.
#
# This file is provided to you under the Apache License,
# Version 2.0 (the "License"); you may not use this file
# except in compliance with the License.  You may obtain
# a copy of the License at
#
#   http:#www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
#
# -------------------------------------------------------------------

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "ubuntu/trusty64"

  config.vm.provider :virtualbox do |vbox|
    vbox.customize ["modifyvm", :id, "--memory", 1024]
  end

  # master => 192.168.50.10
  # slave1 => 192.168.50.11
  # slave2 => 192.168.50.12
  (10..12).each do |i|
    config.vm.define "redis-#{i}" do |redis|
      redis.vm.network :forwarded_port, guest: 6379, host: 6379 + (i - 10)
      redis.vm.network "private_network", ip: "192.168.50.#{i}"

      redis.vm.provision "shell" do |s|
        s.path = "init.sh"
        if i == 10 then
          s.args = ["master", ""]
        else
          s.args = ["slaveof", "192.168.50.10"]
        end
      end
    end
  end
end
