```matlab
%dungBeetleAlgorithm
function [best_position, best_score, beetle_positions, best_scores] = dungBeetleAlgorithm(func, dim, bounds, num_beetles, max_iter)
    % 初始化蜣螂的位置
    beetles = zeros(num_beetles, dim);
    for i = 1:dim
        beetles(:, i) = bounds(i, 1) + (bounds(i, 2) - bounds(i, 1)) * rand(num_beetles, 1);
    end

    best_position = [];
    best_score = inf;
    beetle_positions = cell(num_beetles, 1);
    best_scores = zeros(max_iter, 1);

    for iter = 1:max_iter
        for j = 1:num_beetles
            score = func(beetles(j, :));
            if score < best_score
                best_score = score;
                best_position = beetles(j, :);
            end
            
            % 记录蜣螂位置
            beetle_positions{j} = [beetle_positions{j}; beetles(j, :)];

            % 更新蜣螂位置
            step_size = rand(1, dim);
            direction = randi([0, 1], 1, dim) * 2 - 1; % -1 或 1
            beetles(j, :) = beetles(j, :) + step_size .* direction;
            for i = 1:dim
                if beetles(j, i) < bounds(i, 1)
                    beetles(j, i) = bounds(i, 1);
                elseif beetles(j, i) > bounds(i, 2)
                    beetles(j, i) = bounds(i, 2);
                end
            end
        end
        % 记录当前迭代的最佳得分
        best_scores(iter) = best_score;
        fprintf('Iteration %d: Best Score = %f\n', iter, best_score);
    end
end

```

```matlab
% 定义Rosenbrock函数
rosenbrock = @(x) (1 - x(1))^2 + 100 * (x(2) - x(1)^2)^2;
```

```matlab
%main函数
function main
    % 定义Rosenbrock函数
    rosenbrock = @(x) (1 - x(1))^2 + 100 * (x(2) - x(1)^2)^2;

    % 定义搜索空间
    bounds = [-5, 5; -5, 5];

    % 参数设置
    num_beetles = 50;
    max_iter = 100;
    dim = 2;

    % 运行蜣螂算法
    [best_position, best_score, beetle_positions, best_scores] = dungBeetleAlgorithm(rosenbrock, dim, bounds, num_beetles, max_iter);

    fprintf('Best Position: [%f, %f]\n', best_position(1), best_position(2));
    fprintf('Best Score: %f\n', best_score);

%     % 绘制Rosenbrock函数的等高线图
%     x1 = linspace(bounds(1,1), bounds(1,2), 100);
%     x2 = linspace(bounds(2,1), bounds(2,2), 100);
%     [X1, X2] = meshgrid(x1, x2);
%     Z = (1 - X1).^2 + 100 * (X2 - X1.^2).^2;
% 
%     figure;
%     contour(X1, X2, Z, logspace(-2,3,20), 'LineWidth', 1);
%     hold on;

    % 绘制蜣螂算法搜索路径
    for j = 1:num_beetles
        plot(beetle_positions{j}(:,1), beetle_positions{j}(:,2), '-o', 'DisplayName', sprintf('Beetle %d', j));
    end
    colormap hsv;
    colorbar;
    title('Rosenbrock Function and Dung Beetle Algorithm Search');
    xlabel('x1');
    ylabel('x2');
    legend show;
    hold off;

    % 绘制目标函数值随迭代次数变化的曲线
    figure;
    plot(1:max_iter, best_scores, 'LineWidth', 2);
    title('Objective Function Value vs. Iteration');
    xlabel('Iteration');
    ylabel('Best Score');
    grid on;
end

```