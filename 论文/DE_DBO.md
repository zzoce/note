```
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