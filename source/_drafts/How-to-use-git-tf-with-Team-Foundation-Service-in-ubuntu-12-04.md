title: How to use git tf with Team Foundation Service in ubuntu 12.04
id: 65
categories:
  - Uncategorized
tags:
---

Team Foundation Service is a very useful service provided by Microsoft. You can consider Team Foundation Service as hosted Team Foundation Server. TFS is designed as a cross platform service. In Windows, it works great with Visual Studio. But how can we use TFS in linux and Mac? We have git-tf.
<pre>sudo apt-get install git unzip python-software-properties</pre>
<pre>sudo add-apt-repository ppa:webupd8team/java</pre>
<pre>sudo apt-get update
sudo apt-get install oracle-java7-installer</pre>
<pre>wget http://download.microsoft.com/download/A/E/2/AE23B059-5727-445B-91CC-15B7A078A7F4/git-tf-2.0.0.20121030.zip</pre>
<pre>unzip git-tf-2.0.0.20121030.zip
mv git-tf-2.0.0.20121030 /usr/local/lib/git-tf
</pre>
<pre>vim ~/.profile</pre>
<pre>PATH="$PATH:/usr/local/lib/git-tf"</pre>
<pre>source ~/.profile</pre>
<pre>git tf</pre>
<pre>
git config git-tf.server.username fabrikamfiber4@hotmail.com
git config git-tf.server.password mypassword
</pre>

https://tfs.visualstudio.com/en-us/learn/code/use-git-and-vs-with-tfs/
http://www.ubuntugeek.com/how-to-install-oracle-java-7-in-ubuntu-12-04.html