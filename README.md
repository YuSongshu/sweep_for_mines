简易扫雷（操控台版）
队伍成员：何玉淞、余彦龙
分工：
项目整体的设计思路，部分代码（余彦龙）
主体代码，最终代码整理与优化，介绍文档（何玉淞）
核心代码的讲解：
创建面向对象
class SL		（创建面向对象）
def __init__(self, width, height, mines):		（定义及初始化）
    	······
属性
self.width = width				x轴
self.height = height				y轴
self.mines = mines				地雷
self.back = []					隐藏的面板，用于固定格子
self.visible = []					可见的面板，用于人机交互
self.w_cell_lit = []				空格列表，用于揭示方法，防止递归时无                                                             限循环，超出索引深度
self.finish_reveal = []			记录已揭示的格子，避免重复揭示self.game_over = False				
self.win = False
self.first_click = True			首次点击





方法
def initialize_game    		初始化
def place_mines		  		防止地雷并确保第一个揭示的必为空
def display_board		  		游戏面板的打印输出
def reveal_all		  		游戏结束时揭示所有格子
def flagging			  		“插旗子”
def count_surround_mines		计算以所在格子为中心9*9区域的雷数
def count_numbers				将雷数赋给back以便揭示
def reveal						揭示，利用递归实现周边格子的判断与揭示，揭示后判断游戏是否结束
def check_win					判断游戏是否结束
def play						游戏主循环，处理用户输入并控制游戏流程



主函数
def choose_difficulty		开始时先选择难度
def main						游戏主循环入口
if __name__ == "__main__"	执行 main() 函数
    main()						




核心代码的讲解
self.back = [[' ' for _ in range(self.height)] for _ in range(self.width)]				创建了一个二维列表每一个元素都是‘ ’

mine_positions = random.sample(positions, self.mines)
for x, y in mine_positions:		随机定义地雷位置






def reveal(self, x, y):
    if not (0 <= x < self.width and 0 <= y < self.height):
        return True					用于检查坐标(x,y)是否在有效范围内
    if self.first_click:
        self.place_mines(x, y)
        self.first_click = False		确保第一次为空并保证只有第一次揭示如此

    if self.back[x][y] == 'X' and self.visible[x][y] != 'F':
        self.visible[x][y] = 'X'
        self.game_over = True
        return False

    if self.finish_reveal[x][y] == '1':	   重复揭示函数停止调用重新进入循环
        return True

    if self.back[x][y] != ' ' and self.back[x][y] != 'X':
        self.visible[x][y] = self.back[x][y]
        self.finish_reveal[x][y] = '1'

    if self.visible[x][y] == 'F':
        print("已标记")
        return True

    if self.back[x][y] == ' ':		        
self.visible[x][y] = 'w'
        self.w_cell_lit[x][y] = '0'
        self.finish_reveal[x][y] = '1'
        for dx in [-1, 0, 1]:
            for dy in [-1, 0, 1]:
                nx, ny = x + dx, y + dy
                if 0 <= nx < self.width and 0 <= ny < self.height:
                    if dx != 0 or dy != 0:
                        if self.w_cell_lit[nx][ny] != '0':
                            self.reveal(nx, ny)
                        elif self.back[nx][ny] == 'X':
                            pass
    self.check_win()
    return True
应用递归实现展开周边格子并揭示
返回True函数停止调用，返回False会跳出循环
