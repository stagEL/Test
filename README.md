# Test
CRUD example

Starting to use GitHub

public class Solution {
    public static volatile List<Person> allPeople = new ArrayList<Person>();

    static {
        allPeople.add(Person.createMale("Иванов Иван", new Date()));  //сегодня родился    id=0
        allPeople.add(Person.createMale("Петров Петр", new Date()));  //сегодня родился    id=1
    }

    public static void main(String[] args) throws ParseException {
        //start here - начни тут
        switch (args[0]) {
            case "-c":
                synchronized (allPeople) {
                    for (int i = 1; i < args.length; i += 3) {
                        multipleCreation(args[i], args[i + 1], args[i + 2]);
                    }
                    break;
                }
            case "-u":
                synchronized (allPeople) {
                    for (int i = 1; i < args.length; i += 4) {
                        update(args[i], args[i + 1], args[i + 2], args[i + 3]);
                    }
                    break;
                }
            case "-d":
                synchronized (allPeople) {
                    for (int i = 1; i < args.length; i++) {
                        deletion(args[i]);
                    }
                    break;
                }
            case "-i":
                synchronized (allPeople) {
                    for (int i = 1; i < args.length; i++) {
                        printInfo(args[i]);
                    }
                }
                break;
        }
    }

    public static void multipleCreation(String name, String sex, String birthDate) throws ParseException { //создаем людей
        SimpleDateFormat simpleDateFormat1 = new SimpleDateFormat("dd/MM/yyyy", Locale.ENGLISH);
        if (sex.equals("м")) {
            Person person = Person.createMale(name, simpleDateFormat1.parse(birthDate));
            allPeople.add(person);
            System.out.println(allPeople.indexOf(person));
        } else if (sex.equals("ж")) {
            Person person = Person.createFemale(name, simpleDateFormat1.parse(birthDate));
            allPeople.add(person);
            System.out.println(allPeople.indexOf(person));
        }

    }

    public static void update(String id, String name, String sex, String birthDate) throws ParseException { //обновляем данные
        Person person = allPeople.get(Integer.parseInt(id));
        SimpleDateFormat simpleDateFormat1 = new SimpleDateFormat("dd/MM/yyyy", Locale.ENGLISH);
        person.setName(name);
        if (sex.equals("м")) {
            person.setSex(Sex.MALE);
        } else if (sex.equals("ж")) {
            person.setSex(Sex.FEMALE);
        }
        person.setBirthDate(simpleDateFormat1.parse(birthDate));
    }

    public static void deletion(String id) { //удаляем людей (не меняя размер листа)
        Person person = allPeople.get(Integer.parseInt(id));
        person.setName(null);
        person.setSex(null);
        person.setBirthDate(null);
    }

    public static void printInfo(String id) {
        Person person = allPeople.get(Integer.parseInt(id));
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("dd-MMM-yyyy", Locale.ENGLISH);
        if (person.getSex().equals(Sex.MALE)) {
            System.out.println(person.getName() + " м " + simpleDateFormat.format(person.getBirthDate()));
        } else if (person.getSex().equals(Sex.FEMALE)) {
            System.out.println(person.getName() + " ж " + simpleDateFormat.format(person.getBirthDate()));
        }
    }

}
