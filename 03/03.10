sealed class Figure(val property: Double)

data class Circle(val radius: Double) : Figure(radius)
data class Square(val sideLength: Double) : Figure(sideLength)

enum class Operation {
    INSERT, GET_AREA, GET_PERIMETER, EXIT
}

class BadPropertyException(message: String) : Exception(message)
class WrongOperationTypeException(message: String) : Exception(message)
class WrongFigureTypeException(message: String) : Exception(message)

interface ConsoleService {
    fun work()
}

interface FigureService {
    fun addSquare(property: Double)
    fun addCircle(property: Double)
    fun getPerimeter(): List<Double>
    fun getArea(): List<Double>
}

class FigureServ : FigureService {
    private val figures = mutableListOf<Figure>()

    override fun addSquare(property: Double) {
        if (property <= 0 || property.isNaN()) throw BadPropertyException("Invalid property value: $property")
        figures.add(Square(property))
        println(Square(property).toString())
    }

    override fun addCircle(property: Double) {
        if (property <= 0 || property.isNaN()) throw BadPropertyException("Invalid property value: $property")
        figures.add(Circle(property))
        println(Circle(property).toString())
    }

    override fun getPerimeter(): List<Double> {
        return figures.map {
            when (it) {
                is Circle -> 2 * Math.PI * it.radius
                is Square -> 4 * it.sideLength
            }
        }
    }

    override fun getArea(): List<Double> {
        return figures.map {
            when (it) {
                is Circle -> Math.PI * it.radius * it.radius
                is Square -> it.sideLength * it.sideLength
            }
        }
    }
}

class ConsoleServ(private val figureService: FigureService) : ConsoleService {
    override fun work() {
        while (true) {
            try {
                println("Введите тип операции:\n1) добавить фигуру\n2) получить площадь всех фигур\n3) получить периметр всех фигур\n4) завершить выполнение")
                val operation = getOperation(readln())
                when (operation) {
                    Operation.INSERT -> addFigure()
                    Operation.GET_AREA -> getArea()
                    Operation.GET_PERIMETER -> getPerimeter()
                    Operation.EXIT -> break
                }
            } catch (e: BadPropertyException) {
                println("Введено неверное значение параметра property: ${e.message}")
            } catch (e: WrongOperationTypeException) {
                println("Введен неизвестный тип операции: ${e.message}")
            } catch (e: WrongFigureTypeException) {
                println("Введен неизвестный тип фигуры: ${e.message}")
            }
        }
    }

    private fun getOperation(input: String): Operation {
        return when (input) {
            "1" -> Operation.INSERT
            "2" -> Operation.GET_AREA
            "3" -> Operation.GET_PERIMETER
            "4" -> Operation.EXIT
            else -> throw WrongOperationTypeException(input)
        }
    }

    private fun addFigure() {
        println("Введите тип фигуры:\n1) Круг\n2) Квадрат")
        val figureType = readln()
        println("Введите значение property:")
        val property = readln().toDoubleOrNull() ?: throw BadPropertyException("NaN")

        when (figureType) {
            "1" -> figureService.addCircle(property)
            "2" -> figureService.addSquare(property)
            else -> throw WrongFigureTypeException(figureType)
        }
    }

    private fun getArea() {
        val areas = figureService.getArea()
        println("Площади всех фигур: $areas")
    }

    private fun getPerimeter() {
        val perimeters = figureService.getPerimeter()
        println("Периметры всех фигур: $perimeters")
    }
}

fun main() {
    val figureService = FigureServ()
    val consoleService = ConsoleServ(figureService)
    consoleService.work()
}
