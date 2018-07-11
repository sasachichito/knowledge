```
import java.util.HashMap;
import java.util.Map;

public class Main2 {
	/**
	 * 全てのシステム(アプリケーション)は
	 * 定期的に実行される「BATCH」か、
	 * UIや別システムからリクエストされる「API」のどちらかである。
	 * 我々が作成したアプリケーションがそのどちらでも、始まりはパラメータを受け取るだけだ。
	 * パラメータには、APIならURLが、BATCHならタスク名が渡されるだろう。
	 * アプリケーションはパラメータに対応する処理を実行する。
	 * これが全て。
	 * フレームワークを使うとパラメータと処理の紐付けが自動化されていることが多い。
	 * 今回はフレームワークを利用しない。
	 * フレームワークに隠されているが、全ての始まりはこのmainメソッドだ。
	 * なぜならそれがJVMの仕様だから。
	 * mainメソッドがアプリケーションを起動する。
	 * @param args
	 */
	public static void main(String[] args) {
		Application payrollApplication = new PayrollApplication(); //　給与アプリケーション
		for (String source : args) {
			payrollApplication.setSource(source);
		}
	}
	
	abstract class Application {
		public abstract void setSource(String source);
	}
	
	class PayrollApplication extends Application{
		public void setSource(String source) {
			
		}
	}
	
	class Employee {
		private int empId;
		private PaymentClassification itsPaymentClassification; // 給与分類
		private PaymentMethod itsPaymentMethod; // 支払い方法
		private Affiliation itsAffiliation; // 所属する組合
		
		public Employee(int empId) {
			this.empId = empId;
		}
		
		public void payDay(PayCheck pc) {
			
		}
	}
	
	interface PayCheck {
		// 
	}
	
	interface PaymentClassification {
		abstract double calculatePay(PayCheck pc);
	}
	
	class SalariedClassification implements PaymentClassification {
		public double calculatePay(PayCheck pc) {
			return -1;
		}
	}
	
	class HourlyClassification implements PaymentClassification {
		public double calculatePay(PayCheck pc) {
			return -1;
		}
	}
	
	class CommissionedClassification implements PaymentClassification {
		public double calculatePay(PayCheck pc) {
			return -1;
		}
	}
	
	interface PaymentMethod {
		
	}
	
	interface Affiliation {
		
	}
	
	interface Transaction {
		void execute();
	}
	
	static class PayrollDatabase {
		private static Map<Integer, Employee> itsEmployees = new HashMap<Integer, Employee>();
		
		public static void AddEmployee(int empId, Employee e) {
			itsEmployees.put(empId, e);
		}
		
		public static Employee GetEmployee(int empId) {
			return itsEmployees.get(empId);
		}
	}
	
	
}
```
