```matalb
% differential_evolution
function [best_x, best_fval, history, pop_hist] = differential_evolution(F, dim, NP, max_iter, F_weight, CR)

    % 初始化种群
    pop = rand(dim, NP);  % 在 [0, 1] 内随机初始化种群
    pop = pop * 200 - 100;  % 映射到 [-100, 100] 范围内
    
    % 保存每次迭代的种群
    pop_hist = zeros(dim, NP, max_iter);

    % 评估初始种群
    fvals = zeros(1, NP);
    for i = 1:NP
        fvals(i) = F(pop(:, i));
    end

    % 记录找到的最佳解
    [best_fval, best_idx] = min(fvals);
    best_x = pop(:, best_idx);
    
    % 初始化历史记录，用于存储每次迭代的最佳目标函数值
    history = zeros(1, max_iter);
    
    % 差分进化算法主循环
    for iter = 1:max_iter
        for i = 1:NP
            % 变异操作
            r = randperm(NP, 3);
            r1 = r(1); r2 = r(2); r3 = r(3);
            mutant = pop(:, r1) + F_weight * (pop(:, r2) - pop(:, r3));

            % 交叉操作
            j_rand = randi(dim);
            trial = pop(:, i);
            for j = 1:dim
                if j == j_rand || rand <= CR
                    trial(j) = mutant(j);
                end
            end
            
            % 确保试验向量在 [-100, 100] 范围内
            trial = max(min(trial, 100), -100);

            % 选择操作
            f_trial = F(trial);
            if f_trial < fvals(i)
                pop(:, i) = trial;
                fvals(i) = f_trial;
                if f_trial < best_fval
                    best_x = trial;
                    best_fval = f_trial;
                end
            end
        end
        
        % 记录当前迭代的最佳目标函数值
        history(iter) = best_fval;
        
        % 记录当前种群
        pop_hist(:, :, iter) = pop;
        
        % 显示迭代信息（可选）
        disp(['第 ', num2str(iter), ' 次迭代：最佳目标函数值 = ', num2str(best_fval)]);
    end

end

```

```matlab
%Rosenbrock函数
function o = Rosenbrock(x)
    dim = length(x);
    o = sum(100 * (x(2:dim) - (x(1:dim-1) .^ 2)) .^ 2 + (x(1:dim-1) - 1) .^ 2);
end

```

```matlab
%main函数
% 设置算法参数
dim = 2;       % 问题的维度（对于绘图，使用二维）
NP = 50;       % 种群大小
max_iter = 100; % 最大迭代次数
F_weight = 0.8; % 变异权重 (F)
CR = 0.9;      % 交叉概率 (CR)

% 定义目标函数 Rosenbrock
F = @(x) Rosenbrock(x);

% 运行差分进化算法
[best_x, best_fval, history, pop_hist] = differential_evolution(F, dim, NP, max_iter, F_weight, CR);

% 绘制 Rosenbrock 函数的三维图和等高线图


% 三维图
figure;
[x1, x2] = meshgrid(-5:0.1:5, -5:0.1:15);
z = arrayfun(@(x, y) Rosenbrock([x; y]), x1, x2);
surf(x1, x2, z, 'EdgeColor', 'none');
xlabel('x1');
ylabel('x2');
zlabel('f(x)');
title('Rosenbrock 函数的三维图');
% hold on;
% plot3(best_x(1), best_x(2), best_fval, 'ro', 'MarkerSize', 10, 'LineWidth', 2);
% hold off;
% 
% % 等高线图
% figure;
% contour(x1, x2, z, 50);
% xlabel('x1');
% ylabel('x2');
% title('Rosenbrock 函数的等高线图');
% hold on;
% % 绘制搜索过程
% for iter = 1:max_iter
%     pop = pop_hist(:, :, iter);
%     plot(pop(1, :), pop(2, :), 'b.');
%     pause(0.1); % 暂停以便观察搜索过程
% end
% hold off;

% % 绘制最佳解
% plot(best_x(1), best_x(2), 'ro', 'MarkerSize', 10, 'LineWidth', 2);
% hold off;
% 
% % 显示结果
% disp('最佳解:');
% disp(best_x);
% disp(['最小目标函数值: ', num2str(best_fval)]);

% 绘制目标函数值随迭代次数变化的曲线
figure;
plot(1:max_iter, history, 'LineWidth', 2);
xlabel('迭代次数');
ylabel('目标函数值');
title('目标函数值随迭代次数变化的曲线');

```