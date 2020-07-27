# WAS(Tomcat) ���μ����� ���� �ʴ� ����

> shutdown.sh�� �����ص� ���μ����� �����ʰ� �����ִ� ����

---------------------------------------------------------------------------------------

### 1. ����
- ��ġ ���� ������ �ö��� �ʴ� ���� �߻� 

### 2. ����
- WAS�� shutdown ��Ų �� �����Ŀ� startup�� ��Ű�µ� ���μ����� �����ʰ� ��� �����־� �޸𸮿����� ����־� �޸�Ǯ�� �߻��� ��

### 3. ���
> 1) kill ��ɾ�� �ش� ���μ����� ������ ����ó�� -> �Ź� ������ pid�� ã�� �����ؾ��ϸ� �Ǽ��� Ÿ�ý����� ���μ��� ���� �� ���� �߻� -> ���赵�� ŭ

> 2) startup �� �� �ش� ���μ��� id�� �����  �� pid���� ������ shutdown��Ű�� �Ź� pid���� ã�Ƽ� �������� ��Ű�� �ʾƵ� ��

### 4. �ذ���
- 3-2�� ����� ����

- TOMCAT_HOME/bin/startup.sh ����

- exec "$PRGDIR"/"$EXECUTABLE" start "$@"  ��  �ٷ� ��
export CATALINA_PID=/home/server/tomcat/bin/catalina.pid �߰�

- �� �ɼǰ��� �߰��ϰ� startup.sh�� ���� �� CATALINA_PID�� ����Ǿ� �ش� ��ο� catalina.pid ������ ����Ǿ��ٰ� ����

- TOMCAT_HOME/bin/shutdown.sh ����

- exec "$PRGDIR"/"$EXECUTABLE" stop "$@" �� �ٷ� ��
export CATALINA_PID=/home/server/tomcat/bin/catalina.pid �߰�

- exec "$PRGDIR"/"$EXECUTABLE" stop "$@" -> exec "$PRGDIR"/"$EXECUTABLE" stop -force "$@"�� ����

### 5.��
- shutdown�ÿ� �����ִ� ���μ����� �� ����Ǿ� ������� �޸� ������ ��� �ִ� �뷮�� 30%���ؿ� �ӹ��� 

### 6.���
- startup, shutdown �� CATALINA_PID �ɼ� ���� �߰� �ϱ� ���� ���� 3~5�� ���� �ɸ����� �ֱ������� ��������ʴ� ���μ������� ���� ���������ߵǾ��� �Ϳ� ���� �ξ� ������.  