# require(_once) or include(_once) WILL USE and ONLY use the same WORK DIR as the php script file you request unless you manually called chdir() function
**So you better use absolute path to require or include other php script file in your request-php inclueded(required) php script file, otherwise, it have chance to raise a file doesn't exist fatal error(when using require(_once)) or a file doesn't exist warning(when using include(_once))**